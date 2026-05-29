# Deliverable Spec · 30_gobernanza/

> Defines what the Governance Agent must produce. The most distinctive layer of the framework.

---

## Mandatory outputs (foundation)

### 1. `30_gobernanza/SCHEMA.md`

The graph contract. **Adapt** to project context.

**Length:** ~300-500 lines.

**Required sections:**
1. Design principles (7+).
2. Reference registries (data zones, technical layers, projects, horizons, systems — adapted to project).
3. Common node structure (id, type, name, status, owner, mapeo, version, updated).
4. 14 node types (12 standard + `prompt` + `evaluacion`).
5. Facet vocabularies (status, sensibilidad, ai_act_tier, etc.).
6. Edge types (lineage, consume, governance, authorization, etc.).
7. Adjunct registries (model cards, agent specs, EIPD, FRIA, Art. 30).
8. Repo organization.
9. Validation rules (12+).
10. Examples (2-3 worked).
11. CHANGELOG.

### 2. `30_gobernanza/LOOP.md`

The 5-step operational protocol. Copy from common framework; adapt examples.

**Length:** ~250-400 lines.

### 3. `30_gobernanza/CLAUDE.md`

Context for Claude Code (or any AI agent that will work on the project's code).

**Length:** ~150-300 lines.

**Required sections:**
- What this layer is.
- Required reading at session start.
- When the loop applies (blocking vs flexible).
- How to apply policies when generating code.
- Coupling with SDD (the golden rule).
- Project-specific notes (e.g., "the Master Catalog is methodologically owned by {consultant}").

### 4. `30_gobernanza/STACK_NORMATIVO.md`

Project-specific regulatory stack analysis.

**Length:** ~200-500 lines depending on regulation density.

**Required sections:**
1. Why this stack (not another).
2. Per applicable regulation:
   - State (in force, in transition, deferred).
   - Classification of the project under this regulation.
   - Derived obligations.
   - Compliance dates.
   - Policies that implement.
3. RGPD applicability (if EU + PII).
4. How to apply this stack.
5. Stack decisions (dec-NNN nodes).

**Stack composition rules (proportional):**
- **Always include**: GDPR if EU and PII processed.
- **Recommended for LLM use**: + OWASP LLM Top 10.
- **Recommended for agentic systems**: + OWASP Agentic Top 10.
- **Required for EU AI deployer with user-facing AI**: + AI Act Art. 50 transparency.
- **Add on request only**: ISO 42001, NIST AI RMF, sector-specific (HIPAA, PCI-DSS).
- **Never include** "all of the above" without justification.

### 5. `30_gobernanza/README.md`

Index of the layer.

---

## Required: reference registries

In `30_gobernanza/referencia/`:

### 6. `zonas.yaml`

Data zones Z1..Z4 adapted to project context. Example axes:

- **For clinical data** (Mensalus case): Z1 identity, Z2 knowledge, Z3 clinical (Art. 9), Z4 anonymized research.
- **For corporate confidentiality** (OCA-BIM case): Z1 identity, Z2 methodology catalog, Z3 raw transcripts (contractual confidentiality), Z4 computed aggregates.
- **For financial data**: adapt accordingly.

### 7. `capas.yaml`

Technical layers from SDD §2. Typically C1 frontend, C2 edge, C3 API gateway, C4 application services, C5 data, C6 external. Adapt names/numbers as needed.

### 8. `horizontes.yaml`

Conceptual horizons grouping milestones. Typically H1, H2, H3.

### 9. `proyectos.yaml`

Milestones from `plans/current.md` as governance units (`PRJ-H9`..`PRJ-H16` or equivalent).

### 10. `sistemas.yaml`

Systems from the project's stack with `estado: activo|previsto` and `desde: H{N}`.

---

## Required: decision nodes

In `30_gobernanza/catalogo/decisiones/`:

### 11. dec-001 (meta)

`dec-001-adoptar-gobernanza-como-codigo.md` — always present. Explains why the project adopts this framework.

### 12. dec-002 (meta)

`dec-002-stack-normativo-proporcional.md` — always present. Explains the regulatory stack choice.

### 13. dec-XXX (ADR mirrors)

One file per ADR from SDD §8, named `dec-{NNN}-{slug}.md`. Each contains:

```markdown
---
id: dec-{NNN}-{slug}
tipo: decision
nombre: {Title from ADR}
descripcion: {1-line description}
estado: aceptada | propuesta | sustituida | revertida
propietario: {Tech lead}
mapeo: { capa: ..., proyecto: ..., horizonte: ..., sistema: ... }
horizonte: H{N}
espeja_adr: ADR-{NNN}
supersede: null
superseded_by: null
afecta_a: [list of impacted nodes]
version: 1.0
actualizado: YYYY-MM-DD
---

# dec-{NNN} · {Title}

## Context
...

## Decision
...

## Consequences in governance
...

## Review
...
```

### 14. dec-XXX (assumption mirrors)

One file per assumption from `plans/current.md` §9 (A1..A_N). Same format as ADR mirrors but `espeja_adr: null`.

---

## Required: initial policies and norms

### 15. `30_gobernanza/politicas/` (≥3 policies)

Typical starters (project-dependent):
- `pol-constitutional-retrieval.md` if LLMs used.
- `pol-multi-tenant-app-scoping.md` if multi-tenant.
- `pol-no-entrenamiento-proveedor.md` if external LLM with confidential data.
- `pol-audit-append-only.md` if audit trail required.
- `pol-disclaimer-uso-ia.md` if AI Act Art. 50 applies.
- (more derived from ADRs)

### 16. `30_gobernanza/normas/` (≥1 norm)

One per applicable regulation:
- `norm-ai-act-art-50.md` if applicable.
- `norm-rgpd.md` if EU PII.
- `norm-owasp-llm-top10.md` if LLM.
- `norm-owasp-agentic-top10.md` if agents.

---

## Required: semantic

### 17. `30_gobernanza/semantica/dominios.yaml`

4-6 domains orthogonal to zones.

### 18. `30_gobernanza/semantica/glosario.yaml`

15-30 critical terms with definition and authority source.

---

## Optional: initial catalog seed

Seed `catalogo/datasets/`, `catalogo/modelos/`, `catalogo/agentes/`, `catalogo/flujos/`, `catalogo/casos-uso/` with the obvious entries from product + techspec.

**Rule of thumb:** seed 3-7 of each type for a project of medium complexity. The LOOP grows the rest.

---

## Acceptance criteria

- [ ] SCHEMA.md has 12+ validation rules.
- [ ] Zones in `zonas.yaml` are project-specific (not OCA-BIM copy).
- [ ] STACK_NORMATIVO.md doesn't include irrelevant regulations.
- [ ] Every ADR in SDD §8 has a matching `dec-` node.
- [ ] Every `dec-` has `espeja_adr` populated.
- [ ] ≥3 policies in `politicas/`.
- [ ] ≥1 norm in `normas/`.
- [ ] `glosario.yaml` has ≥10 terms.
- [ ] User approved.

---

## Common mistakes to avoid

- **Copying OCA-BIM zones literally** for a non-clinical project.
- **Over-engineering** stack normativo with ISO 42001 + NIST when project is small EU SaaS.
- **Inventing policies** that don't derive from an ADR or a norm.
- **Empty catalog seed**: at least seed the obvious datasets/models/agents from brief + Producto.
- **Skipping dec-001 and dec-002**: these meta-decisions must always be present.
