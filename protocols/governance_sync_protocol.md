# Protocol — Governance sync per commit

> **MANDATORY**. This protocol governs how every agent keeps the governance layer (`30_gobernanza/`) in sync with code changes, schema changes, agent definitions, and decisions, **in the same commit** as the change itself.
>
> The rule was formalized after the OCA-BIM H10 saneamiento (2026-05-29) showed that even 4 commits of drift cost ~30 min to recover. Drift past 5-6 commits is irrecoverable without a dedicated audit pass.

---

## Why this exists

Governance-as-code only works if the two layers move together. When code lands without its governance update:

1. The runtime state of `agt-*` nodes drifts from reality (they describe v0.3 schema while the code runs v0.5).
2. The `ds-*` schema blocks lose track of column additions / drops.
3. The `pol-*` enforcement notes become aspirational instead of describing what's actually wired.
4. A regulatory auditor reading only `30_gobernanza/` sees a system that doesn't exist.

After 5 commits like this, a saneamiento costs **proportionally more than just doing it right each time**.

---

## The rule

**Every commit that touches code, schema, prompts, or API surface MUST also touch the affected governance nodes in the same commit.**

Not "in the next commit". Not "at end of milestone". Not "we'll batch the sync later". **Same commit.**

If the developer agent cannot identify which nodes to touch, it escalates to the user via `escalation_protocol.md` rather than letting the drift accumulate.

---

## What to touch — mapping table

| You changed… | You must update… |
|---|---|
| New / renamed DB table or column | `30_gobernanza/catalogo/datasets/ds-*.md` — bump `esquema:` block + `version:` + `actualizado:` |
| New / changed AI agent (LLM call, prompt template, retrieval logic) | `catalogo/agentes/agt-*.md` — bump `estado_implementacion:`, version |
| New flow (pipeline, publication, ingestion, discovery) | `catalogo/flujos/flw-*.md` — update diagram + `gobernado_por:` |
| New endpoint with capability gate | `politicas/pol-*.md` of the relevant policy — confirm gate is documented |
| New auth mechanism, role mapping, tenant scoping | `catalogo/decisiones/dec-*.md` — bump version with new tenant/role data |
| New domain-level term (TenantScope, KPI pair, working state, etc.) | `semantica/glosario.yaml` — add `term-*` entry |
| New domain or sub-domain | `semantica/dominios.yaml` — add `dom-*` entry |
| New external service / API surface | `catalogo/sistemas/sys-*.md` — new node or bump |
| New external secret / credential (publishable id, JWKS URL, …) | `dec-*` for the provider + secret-handling policy |
| Drop of legacy schema / API | Affected `agt-*` / `ds-*` / `flw-*`: add `estado_implementacion.legacy_dropped_in: <commit-hash-or-tag>` |

---

## How to update each node type

### Datasets (`ds-*`)

Add or update an `esquema:` block under the front-matter:

```yaml
esquema:
  schema_version: 2.0.0
  jerarquia: cluster → behavior → kpi → linguistic_signal
  m_n: behavior_applicable_routines
  cambio_v0_3_v0_5: |
    Scenarios eliminados; clusters como nivel superior;
    KPI cuelga del Behavior con valence anchor/blocker;
    signals cuelgan del KPI (no del Behavior);
    routines como dimensión M:N de aplicabilidad.
  migracion: v05_007_drop_legacy_and_rename (H9 commit 6)
  poblacion: agt-behavior-detector (reescrito en H14)
```

Bump `version:` (semver minor for additive, major for incompatible) and `actualizado:` to today's date.

### Agents (`agt-*`)

Add an `estado_implementacion:` block:

```yaml
estado_implementacion:
  schema_target: v0.5 (kpi_logical_id + signal_logical_id + signal_matched_text)
  schema_actual_codigo: legacy (deshabilitado en H9 commit 6, columnas v0.5 ya añadidas)
  reescritura_en: H14 (pipeline contra v0.5)
```

When the agent goes live (rewrite lands), replace the `schema_actual_codigo` line with `vivo: H14 commit X`.

### Flows (`flw-*`)

Update the diagram in the body to reflect the actual API path + capabilities + audit invariants:

```
POST /api/v1/catalog/publish { target_version: "1.0.0" }
   │  (capability: catalog:publish)
   ↓
Curation gate (pol-mod-08-curation-gate)
   ↓
Symmetry validation (T-9.9)
   ↓
INSERT published_catalog_versions (state=publishing → published)
   ↓
Audit: CatalogAuthoringAudit action='publish'
```

### Policies (`pol-*`)

Add an `implementacion:` block:

```yaml
implementacion:
  helper: app.core.auth_clerk.current_tenant_scope -> TenantScope
  introducido_en: H10 commit 2 (T-10.1, Clerk middleware)
  estado_actual:
    - org_id resuelto desde JWT claims
    - client_id resolution diferida a T-10.5
  scoped_query: pendiente de T-10.5
```

### Decisions (`dec-*`)

When a tenant or service is provisioned, bump the version with a "Tenant provisionado" / "Service provisionado" table:

```markdown
## Tenant provisionado (v1.1 — 2026-05-29)

| Campo | Valor |
|---|---|
| Application name | OCA-BIM |
| Tier | Development |
| Publishable key | pk_test_... |
```

Never write secrets here. Only public identifiers.

### Glossary (`semantica/glosario.yaml`)

Append a `- id: term-xxx` block at the bottom (the dom-* of the term decides where, but order within file is not strict):

```yaml
- id: term-tenant-scope
  definicion: |
    Estructura inmutable construida por el middleware Clerk en cada
    request. Incluye (user_id, org_id, org_role, org_slug,
    capabilities, client_id). Implementada en app/core/auth_clerk.py
    desde H10 commit 2.
  fuente_autoridad: dani
  sinonimos: []
  dominio: dom-identidad
```

### Domains (`semantica/dominios.yaml`)

Append:

```yaml
- id: dom-identidad
  nombre: Identidad y autorización
  descripcion: Cubre Clerk JWT validation, TenantScope, capability tokens.
  padre: dom-plataforma
  fuente_autoridad: dani
  propietario: Dani Cardelús
  estado: activo
```

---

## How to record it in the commit message

Add a `Refs:` or `Gobernanza:` section at the end of the commit message listing every node bumped:

```
feat(api/catalog): endpoints /catalog/* read-only v0.5 (T-10.2)

[body of the message]

Refs:
- docs/20_especificacion_tecnica/sdd.md §4
- docs/20_especificacion_tecnica/plans/current.md §3 H10 T-10.2

Gobernanza (bumpeados en este commit):
- agt-classifier             v0.1 → v0.2
- ds-master-catalog          v0.1 → v0.2
- pol-multi-tenant-app-scoping  v1.0 → v1.1
- glosario.yaml              +9 términos
- dominios.yaml              +2 dominios
```

A future audit can `git log --grep "Gobernanza"` and trace every code↔governance pair.

---

## When you genuinely cannot update governance

There are a few cases where the developer agent legitimately cannot do the bump in the same commit:

1. **You don't know what to bump.** Then escalate via `escalation_protocol.md`. The user (or the Governance Agent) tells you. Do NOT silently skip.

2. **The bump requires a new `dec-` and the user has not approved the underlying decision.** Then the code change itself is premature — pause and escalate first.

3. **The node to bump does not exist yet** because the project is in early stage. Then create it as part of the same commit (governance grows progressively, but not by skipping).

There is no fourth case. "Lo hago al cierre del hito" is the antipattern A17 described in `known_antipatterns.md`.

---

## How Orchestrator enforces this

After each handoff from a developer-flavored agent (TechSpec, MOD-08 author, …), Orchestrator:

1. Runs `git log -1 --stat` on the last commit.
2. Lists the touched paths: are any in `20_especificacion_tecnica/`, `backend/`, `frontend/`?
3. If yes: are any in `30_gobernanza/` in the same commit?
4. If no: reject the handoff with reason `GOVERNANCE_DRIFT`. Send the developer back to update.

This is a hard gate. Override only if the developer files a `dec-` explaining a one-off exception.

---

## TL;DR

> If you change code, you change governance in the same commit. No exceptions, no "later", no "batch sync". The drift cost grows exponentially.

End of protocol.
