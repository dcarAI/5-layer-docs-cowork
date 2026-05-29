# Deliverable Spec · 20_especificacion_tecnica/

> Defines what the TechSpec Agent must produce.

---

## Mandatory outputs

### 1. `20_especificacion_tecnica/sdd.md`

**Purpose:** Software Design Document. Implementation-level contract.

**Length:** typically 1000-4000 lines. OCA-BIM example was 3757 lines.

**Required sections (14):**
1. Introduction (purpose, scope, audiences, glossary, conventions, refs to prior versions).
2. Architectural vision (layered diagram ASCII, modules → packages, deployment, stack per layer, principles).
3. Data model (ER diagram + literal SQL DDL for each table + ENUMs + ID conventions + versioning strategy + indexes + constraints + cross-table invariants).
4. API (REST principles, auth flow, literal JSON contracts per endpoint family, errors, pagination, idempotency, versioning policy).
5. Processes and pipelines (sequence diagrams ASCII, pseudocode for each pipeline, failure isolation, retries).
6. UI / Frontend (architecture, routing, components catalog, typing pipeline, data fetching, i18n, auth client).
7. Auth and tenancy (provider model, JWT flow, tenant scoping, capabilities, dev bypass).
8. **ADRs** — 10-15 Architectural Decision Records with Context / Options / Decision / Consequences / Status format. **Each ADR has a slug.**
9. NFRs (performance, reliability, observability, security, scalability, i18n).
10. Testing strategy (pyramid, backend, frontend, LLM-dependent, test data, CI sequence).
11. Deployment (dev local, staging, migrations strategy, backup, runbook, env vars per env).
12. Migration plan reference (link to `plans/current.md`).
13. Technical risks (top open risks with mitigation).
14. Appendices (glossary, ID conventions, ENUM listing, refs).

**Forbidden:**
- Generating actual application code.
- Product/business decisions (those are in `10_producto/`).
- Regulatory analysis depth (that's `30_gobernanza/STACK_NORMATIVO.md`).

### 2. `20_especificacion_tecnica/plans/current.md`

**Purpose:** implementation plan with numbered milestones.

**Length:** typically 500-1500 lines depending on complexity.

**Required sections:**
0. Executive summary.
1. Current state (snapshot of what's done).
2. Impact map (what survives, what changes, what's added, what's discarded).
3. Milestones (`PRJ-H{N}` or equivalent — each with Scope, Acceptance criteria, Gate, Estimation).
4. Risks and inherited debt.
5. Notes on existing fixtures / legacy.
6. Session roadmap.
7-8. Appendices (entity mapping, endpoint mapping).
9. Explicit assumptions (A1..A_N).

### 3. `20_especificacion_tecnica/plans/README.md`

Short README explaining: `current.md` is the active plan; `archive/v*.md` are superseded. Rules for bumping.

---

## Conditional outputs

### 4. `20_especificacion_tecnica/api_surface.md`

Generate **if the product has an API**. Defines URL structure, principles, endpoint catalog by family, end-to-end flows (4 typical: ingest, primary operation, admin, etc.), error catalog.

### 5. `20_especificacion_tecnica/catalog_schema.md`

Generate **if the product has a structured catalog** (e.g., methodological library, content schema). YAML format definition for bootstrap import or external editing.

### 6. `20_especificacion_tecnica/deployment/staging.md`

Generate **if staging is planned**.

### 7. `20_especificacion_tecnica/deployment/production.md`

Generate **only if production target is defined**. Most MVPs skip this.

### 8. `20_especificacion_tecnica/deployment/README.md`

Required if `deployment/` has any files. Short index.

---

## ADR slug convention

Each ADR in §8 of sdd.md must have a slug, declared at the end of the ADR:

```markdown
### ADR-NNN — {Decision title}

...

**Slug:** {kebab-case-slug}
```

These slugs are used by Governance Agent to name `dec-NNN-{slug}.md`.

---

## Acceptance criteria

- [ ] All 14 SDD sections present.
- [ ] §3 has SQL DDL for every table; no "TBD" or "..." placeholders.
- [ ] §4 has literal JSON examples (request + response) for at least 5 endpoints.
- [ ] §8 has ≥5 ADRs; each with slug.
- [ ] §10 testing strategy non-empty.
- [ ] `plans/current.md` exists with ≥3 numbered milestones.
- [ ] Each milestone has acceptance criteria.
- [ ] User approved.

---

## Common mistakes to avoid

- **Skipping SQL DDL.** Producing only Python class definitions or pseudo-ER diagrams is insufficient. SQL DDL is the contract.
- **Hand-waving ADRs.** Don't propose "Option A" without trade-offs.
- **Mixing layers.** Product value statements stay in `10_producto/`. Regulatory frameworks in `30_gobernanza/`.
- **Inventing constraints.** If the brief didn't say "<2s p95 latency", don't invent it. Ask user.
- **Generating code.** Pseudocode is OK to clarify a pipeline. Actual implementation Python files are not the deliverable.
