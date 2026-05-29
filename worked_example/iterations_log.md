# Worked example — Iterations log (OCA-BIM)

> The loops backward that actually happened during the OCA-BIM project. Shows that iteration is normal and the framework absorbs it.

---

## Iteration 1 — P2 → P1 (May 5 → May 6)

**Trigger:** Producto Agent, while drafting `product_detail.md`, realized that the "Manuel Macedo as methodology owner" had implications for audiences (he is a primary user but also content authority).

**Action:** Went back to Panorama. Updated `00_panorama/README.md` audiences table to distinguish:
- Manuel as **content authority** (separate from consultant user category).
- Manuel as **expert user** of the Catalog Authoring Workspace.

**Downstream impact:** Producto Agent updated `product_detail.md` §5 Stakeholders accordingly.

**Time cost:** ~20 min.

**Lesson:** Audience categorization isn't always obvious upfront. Iteration is faster than getting it wrong.

---

## Iteration 2 — P3 → P2 (May 8)

**Trigger:** TechSpec Agent, while drafting SDD §3 Data Model, found that `Behavior` in `product_detail.md` was ambiguous — sometimes referred to a behavior pattern (anchor or blocker as separate entities), sometimes to the pair concept (a behavior that has both).

**Action:** Went back to Producto Agent for clarification. Asked the user: "Is `Behavior` the unit (singular, with anchor + blocker as properties) or are anchor and blocker two separate Behaviors?"

**User answer:** Behavior is the unit; anchor and blocker are paired KPIs of that Behavior.

**Action:** Updated `product_elements.yaml` DOM-05 (Behavior) to clarify. Updated `product_detail.md` §3.

**Downstream impact:** TechSpec proceeded with SDD §3 modeling Behavior as a table with a separate `kpis` table where each KPI has a `valence` (anchor or blocker) and `symmetric_to` pointing to the pair.

**Time cost:** ~40 min.

**Lesson:** Domain modeling is where Product and TechSpec friction surfaces. The earlier you clarify, the cleaner the SDD.

---

## Iteration 3 — P3 → P3 (May 15, v0.2 → v0.3 bump)

**Trigger:** Mid-implementation, user (with Manuel) introduced a new domain concept: "Imputed Leakage" — manual monetary input by Manuel during Discovery phase to size opportunity. Plus a 4th confidence tier (`imputed` < estimated < modeled < demonstrated).

**Action:** Created `MIGRATION_TO_v0_3.md` documenting the bump. Updated SDD to add:
- New entity `ImputedLeakage` (DOM-12).
- 4-tier confidence ENUM.
- New submodule MOD-03.SM-04 "Imputed Leakage Capture".
- New ADR (within-profile binding adjustability).

**Downstream impact:** Governance Agent created new `dec-` nodes. New policy `pol-imputed-leakage-blockers-only`. Update `STACK_NORMATIVO.md` to mention transparency tier of the imputed display.

**Time cost:** ~3 hours.

**Lesson:** Architectural bumps mid-implementation happen. Versioning the migration plan (and preserving prior in `archive/`) is essential.

---

## Iteration 4 — P3 → P3 (May 20, v0.3 → v0.4 bump)

**Trigger:** Two significant architectural changes:
1. Manuel requested **LinguisticSignal as first-class entity** (DOM-13) instead of a field on KPI.
2. Decision to introduce **MOD-08 "Master Catalog Authoring"** as a new module upstream of all others.
3. Decision to migrate **catalog from YAML to DB** as single source of truth.
4. Auth provider decision: **Clerk + GCP for staging**.

**Action:** Created `MIGRATION_TO_v0_4.md` (superseding v0.3). Reordered milestones — MOD-08 must come before Discovery. Updated SDD §2 architectural vision, §3 data model, §7 auth.

**Downstream impact:** Governance Agent updated `dec-` nodes for affected ADRs. Created new ones (Clerk auth, GCP staging, DB-as-source-of-truth, DOM-13 first-class, MOD-08 module zero).

**Time cost:** ~5 hours.

**Lesson:** Multiple simultaneous bumps are normal during early implementation. Bumping the migration plan version (v0.4) and adding callout in v0.3 saying "superseded" preserves history.

---

## Iteration 5 — P3 → P2 → P3 → P4 (May 26-28, v0.4 → v0.5 bump)

**Trigger:** Manuel delivered final master catalog matrix (`matrix_26_05_2026.xlsx`). The data model in v0.4 had KPIs at cell level (Behavior × Routine). Manuel's matrix had KPIs at Behavior level with `valence` (anchor or blocker) as a property of each KPI in a symmetric pair.

**Action:** Substantial loop:
1. P2 update: `product_detail.md` DOM-06 (KPI) redefined; `observation_cells` removed; matrix `applicable_routines` introduced on Behavior. `product_elements.yaml` updated.
2. P3 update: SDD §3 schema rewritten. New ADRs (behavior-centric model, KPI valence symmetric, observation_cells deprecated). Schema MAJOR bump from 1.2.0 to 2.0.0.
3. P4 update: New `dec-` mirrors. Updated `referencia/` for new conceptual entities. New policy `pol-imputed-leakage-blockers-only` extended.

**Downstream impact:** Plan `current.md` rewritten as `v0_5.md`. Prior `v0_4.md` moved to `archive/`. Some implementation work (some H9 done) became invalidated — about 40% of v0.4 code survives. Documented as risks in the new plan.

**Time cost:** ~12 hours over 2-3 days.

**Lesson:** Major methodology delivery from external consultant can rewrite the architecture. The framework absorbs this because:
- Brief stayed the same (mission, audiences).
- ADRs are mirrored, so changes propagate to governance.
- Plans are versioned with `archive/`.

---

## Iteration 6 — P5 → P0 (May 29)

**Trigger:** Reference Curator, while organizing materials, realized the brief mentioned `OCA_BIM_Routines_KPIs_Final_valid.xlsx` but the user had **three** Excel files from Manuel. Brief said "an Excel"; reality was three.

**Action:** Back to Discovery briefly to clarify which is current vs historical.

**User clarified:** matrix from May 26 is current; two earlier are inert. Reference Curator moved two earlier to `_archive/` and only matrix_26_05_2026.xlsx is in active path.

**Downstream impact:** Update `dec-016-excel-master-catalog-entregado.md` to note the three-Excel history.

**Time cost:** ~15 min.

**Lesson:** Sub-folder organization in `40_referencia/consultores/{name}/_archive/` is essential when consultants iterate.

---

## Statistics

| Metric | Value |
|---|---|
| Total iterations during the project | 6 documented |
| Backwards loops | All 6 |
| Forward jumps skipped | 0 |
| Phases iterated more than once | P3 (3 times — bumps), P2 (2 times) |
| Average iteration cost | ~3 hours (excluding the big v0.5 loop of 12 hours) |
| % of total project effort spent on iteration | ~20% |

---

## Patterns observed

1. **Most iterations are 1-step backward** (Pn → P(n-1)). Rarely 2+.
2. **Major version bumps trigger multi-step iterations** (P3 → P2 → P3 → P4). Expected and absorbed.
3. **New external material from consultants triggers reorganization** in `40_referencia/`, plus possibly `dec-` mirrors.
4. **Documentation never went stale** because BITACORA was updated at each iteration.

---

End of iterations log.
