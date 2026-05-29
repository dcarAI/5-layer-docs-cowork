# Common Context — shared knowledge base for all agents

> All 7 agents in the team must have access to this file as part of their knowledge base. It defines the framework, conventions, and shared vocabulary they operate on.

---

## The 5-Layer Docs Framework

A pattern for organizing project documentation in product projects assisted by AI. Five layers, each with clear responsibility and directional dependencies.

```
00_panorama/                 🏠 Stable context + living journal (BITACORA)
10_producto/                 📘 What the product does (PM-flavored)
20_especificacion_tecnica/   ⚙️ How it's built internally (SDD + plans)
30_gobernanza/               🛡️ Who governs what, under whose authority
40_referencia/               📎 External material (consultants, demo, history)
50_bitacora_y_revision/      📋 Review tools and traceability
```

### Directional dependencies

```
00_panorama  →  10_producto  →  20_especificacion_tecnica  →  30_gobernanza
                                                                   ↑
                                              40_referencia, 50_bitacora_y_revision
                                              (cross-cutting support)
```

**Rules:**
- A layer can read from layers above it, never below.
- Governance is **transversal** (referenced from all) but only **grows progressively** as the project advances.
- Reference and journal are support layers; they don't contain product truth, they support it.

### Why this exists

Without structure, project docs end up in a flat `docs/` folder where:
- The PM cannot find value proposition among the SQL DDL.
- The engineer wastes time reading stakeholder catalogs.
- The compliance auditor cannot locate where AI Act controls live.
- The new collaborator does not know where to start.

The 5-layer framework solves this by enforcing **separation of concerns** and **visible dependencies**.

---

## Naming conventions

### Folder naming

`{NN}_{name_in_user_language}` where NN is zero-padded:

| Folder | Reserved for |
|---|---|
| `00_panorama/` | Stable context + journal |
| `10_producto/` | Product definition |
| `20_especificacion_tecnica/` | SDD + schemas + technical plans |
| `30_gobernanza/` | Governance as code |
| `40_referencia/` | External data and docs |
| `50_bitacora_y_revision/` | Review and traceability |

**Gaps of 10 are intentional** — allow inserting layers (`05_`, `15_`, `25_`...).

### File naming

- `README.md` is the entry point of each folder.
- `current.md` is the active version of an evolving doc (e.g., implementation plan).
- `archive/{version}.md` for superseded versions.
- For descriptive files: `snake_case.md` or `kebab-case.md`, consistent within a folder.

### Governance node prefixes

```
dec-  decision         pol-  policy            norm- norm/regulation
ds-   dataset          mdl-  model (AI/LLM)    agt-  agent
flw-  flow             task- task              uc-   use case
dom-  domain           term- glossary term
prm-  prompt           eval- evaluation
```

### Versioning

SemVer in document headers/frontmatter. Documents bump version when content changes substantially. Folder names do not include version.

---

## Language conventions

For OCA-BIM and most projects targeted here:

- **Body text, narrative, user-facing docs**: Spanish (or the team's working language).
- **Code, schemas, identifiers, ENUMs, error messages, commits**: English.
- **System prompts of agents**: English (better LLM performance).
- **Document headers, frontmatter keys**: English (interoperability).

**Adapt per project.** If the team works in English natively, all in English. If multilingual, document the rule in `conventions.md` of the target project.

---

## Governance as code

The governance layer (`30_gobernanza/`) is the most distinctive aspect of this framework.

It is **a graph of nodes** stored as markdown files with YAML front-matter, machine-readable and versioned in git. The nodes are:

- **Decisions** (`dec-`): architectural decisions with state (`propuesta` → `aceptada` → `sustituida` → `revertida`) and trajectory (`actual` / `objetivo` / `transicion_en` / `segun`).
- **Datasets, systems, flows, tasks** (`ds-`, `sys-`, `flw-`, `task-`): the operational catalog.
- **Models, agents** (`mdl-`, `agt-`): AI components with risk declared.
- **Use cases** (`uc-`): functional scenarios.
- **Policies** (`pol-`): rules the code respects.
- **Norms** (`norm-`): regulatory references.
- **Domains, glossary** (`dom-`, `term-`): semantic layer.

### The 5-step LOOP

Whenever a new asset, decision, model, agent, or prompt enters the project:

1. **Identify** — assign id, declare type.
2. **Classify** — `mapeo` block + facets (AI Act tier, OWASP, etc.).
3. **Evaluate** — semaphore, risk_ia, EIPD/FRIA triggers.
4. **Register** — commit the node and (if applicable) the corresponding `dec-`.
5. **Control** — derive policy or reference existing one.

**Modes:**
- **Blocking**: for new decisions, models, agents, prompts, Z2/Z3 datasets, new external systems.
- **Flexible** (skip steps 3 and 5 if justified in commit): for routine Z1/Z4 assets following established patterns.

### Coupling with SDD

Every ADR in the SDD §8 has its mirror as a `dec-` node with `espeja_adr: ADR-NNN`. The SDD holds the narrative; the node holds the state, trajectory, and edges.

**Zero content duplication; structural traceability duplication.**

---

## Iteration philosophy

The framework is **not waterfall**. The five-layer flow is the initial direction, but loops happen:

- Discovering a Producto gap may send you back to Panorama.
- TechSpec finding contradiction in Producto may halt and request clarification.
- Governance detecting an ADR without policy may ask TechSpec to clarify first.

The Orchestrator manages these loops.

**Critical:** the team **never improvises silently**. If a decision must be made and the user has not approved it, the team escalates (see `protocols/escalation_protocol.md`).

---

## The worked example: OCA-BIM™

OCA-BIM is the project where this framework was first designed and applied. It is your reference for "what does done look like".

When in doubt about how a layer should look, consult `worked_example/`:

- `intake_session.md` — how the discovery conversation went.
- `iterations_log.md` — the loops that actually happened.
- `final_outputs_map.md` — what each agent generated.

The OCA-BIM project itself is a behavior analysis platform (Leadership Team meetings → behavioral KPIs → P&L impact) but you do not need to know the domain. You need to know **how the docs were structured**.

---

## Cross-cutting principles

1. **Separate concerns** (each layer one responsibility).
2. **Make dependencies visible** (numbering, READMEs per layer).
3. **Preserve history** (`archive/`, `historico/` — never delete).
4. **Governance as code** (machine-readable, consumable by AI agents).
5. **Journal separate from panorama** (stable vs. changing).
6. **External material does not contaminate internal layers** (`40_referencia/` is a paraguas).
7. **Replicable by writing**, not by habit.

---

## Vocabulary (shared)

| Term | Meaning |
|---|---|
| **Brief** | The initial document the Discovery Agent produces from the user conversation. Inputs all other agents. |
| **Canon document** | The authoritative document of a layer (e.g., `product_detail.md` for Producto, `sdd.md` for TechSpec). |
| **Node** | A markdown file in the governance graph (`30_gobernanza/catalogo/*/{prefix}-XXX.md`). |
| **Mirror (espejo)** | A `dec-` node that references an SDD ADR via `espeja_adr`. |
| **State** | The JSON object the Orchestrator maintains describing project progress, outputs, open questions. |
| **Handoff** | The structured transfer of work between agents at phase boundaries. |
| **Escalation** | Stopping work and asking the user for input via `escalation_protocol.md`. |
| **Phase** | One of P0 (Discovery), P1 (Panorama), P2 (Producto), P3 (TechSpec), P4 (Governance), P5 (Reference). |
| **Iteration** | A loop backward to a prior phase to fill a gap or fix a contradiction. |
| **Worked example** | The OCA-BIM project, used as reference. |

---

## What is NOT in scope of any agent

- Generating actual source code (programming).
- Designing UI/UX visual mockups (only structural wireframes if needed).
- Making business decisions (strategy, pricing, partnerships).
- Negotiating with consultants or external parties.
- Anything outside the 5-layer documentation framework.

If a request falls outside, the receiving agent escalates to the user.

---

## End of common context.
