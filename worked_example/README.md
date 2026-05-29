# Worked example — OCA-BIM™

> The 5-Layer Docs Framework was designed, refined, and applied for the first time in the **OCA-BIM™** project (TechBizDesign + AgileFOCUS, May 2026). This folder captures that project as a reference case: how the team conversations went, which loops occurred, and what the final outputs looked like.

---

## What OCA-BIM is

A platform for **detecting, quantifying, and visualizing leadership behaviors in Leadership Team meetings**, based on a proprietary methodology (Behavior Impact Library, BIL) authored by Manuel Macedo (AgileFOCUS).

The system:

1. Takes transcripts of LT meetings as input.
2. Identifies behaviors (anchors and blockers) from a published catalog.
3. Computes behavioral KPIs and monetary impact ranges.
4. Presents results in an executive dashboard with 6 analytical views.

**You do not need to understand this domain.** The point of the worked example is to show **how the documentation was structured**, not what OCA-BIM does.

---

## Why this is useful

When you (or an agent) wonder "what does a well-structured `30_gobernanza/SCHEMA.md` look like?", you check this folder. Same for SDD ADRs, product elements YAML, BITACORA, etc.

The agents read this as part of their `_common_context.md`.

---

## Files in this folder

- **`intake_session.md`** — synthetic transcript of the discovery conversation. Shows how the Discovery Agent extracted the brief over ~20 questions.
- **`iterations_log.md`** — the loops that actually happened during the project (4-6 documented iterations).
- **`final_outputs_map.md`** — what each agent generated. Maps `docs/{folder}/{file}` to the agent that produced it.

---

## Key insight from this case

OCA-BIM evolved through **five major versions** in three weeks (May 5 to May 29, 2026):

- **v0.2.0** (May 15): two-phase architecture introduced.
- **v0.3.0** (May 19): Imputed Leakage, 4-tier confidence, binding adjustability.
- **v0.4.0** (May 20): LinguisticSignal first-class, MOD-08 module zero, DB-as-source-of-truth, Clerk + GCP staging.
- **v0.5.0** (May 28): behavior-centric model with KPI valence symmetric pairs.

Each version bump was **breaking** in some sense, but the structure absorbed it because:

1. Each architectural decision was registered as an ADR.
2. Each ADR was mirrored as a `dec-` node in governance.
3. Plans of implementation were versioned (`current.md` + `archive/v0_3.md`, `v0_4.md`).
4. The brief stayed stable; only downstream artifacts changed.
5. The framework itself (5 layers) was applied retrospectively after these bumps, not before.

**Implication for new projects:** the framework is robust to architectural pivots if you commit to its discipline. Don't fear iteration.

---

## Where to find the real OCA-BIM project

The actual project lives at https://github.com/dcarAI/oca-bim and its `docs/` folder is structured exactly per this framework.

Reference paths to consult when in doubt:

- `oca-bim/docs/00_panorama/README.md` — what a Panorama looks like.
- `oca-bim/docs/10_producto/product_detail.md` — Forrest PMP v0 example.
- `oca-bim/docs/10_producto/product_elements.yaml` — machine-readable elements.
- `oca-bim/docs/20_especificacion_tecnica/sdd.md` — 3757-line SDD example.
- `oca-bim/docs/30_gobernanza/SCHEMA.md` — governance contract.
- `oca-bim/docs/30_gobernanza/STACK_NORMATIVO.md` — proportional stack analysis.
- `oca-bim/docs/30_gobernanza/catalogo/decisiones/` — 24 dec- nodes (13 ADR mirrors + 9 assumption mirrors + 2 meta).

---

## Lessons distilled

1. **Discovery is non-negotiable.** Skipping it produces abstract docs that no one trusts.
2. **The brief is the source of truth.** Disagreements between agents are resolved by going back to the brief.
3. **Governance can be applied retrospectively** (as we did with OCA-BIM, which had H1-H6 done before adopting the framework). But it's easier to apply progressively from the start.
4. **ADRs slug consistently.** Every TechSpec output that introduces an ADR uses a slug; this is the contract with Governance Agent for mirroring.
5. **The framework adapts to consultant relationships.** Manuel Macedo (external methodologist) has a dedicated folder in `40_referencia/consultores/manuel/` with his briefing, deliveries, and archives. Replicate for your consultants.
6. **Proportionality in normative stack.** OCA-BIM is a small consultancy MVP; we chose AI Act Art. 50 + OWASP + GDPR. ISO 42001 and NIST AI RMF deferred to integration phase. Match your project.

---

End of worked example overview.
