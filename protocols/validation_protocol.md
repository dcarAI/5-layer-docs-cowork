# Validation Protocol — cross-layer consistency checks

> Used by the **Orchestrator** at every phase boundary. Defines what to verify before advancing.

---

## Per-phase checklists

### After P0 Discovery → before P1 Panorama

Brief completeness:
- [ ] §1 Identity: name + elevator pitch + owning org present.
- [ ] §2 Mission: at least 2 sentences.
- [ ] §3 Audiences: ≥1 primary user identified.
- [ ] §4 Phase: stated (idea/prototype/MVP/mid/mature).
- [ ] §5 Regulation: explicit (even "no applicable regulation").
- [ ] §6 Tech stack: backend + frontend + db at minimum.
- [ ] §7 Existing materials: list present (can be empty).
- [ ] §8 Roles: technical lead identified.
- [ ] §9 Constraints: present (can be "none").
- [ ] §10 Open questions: list present.
- [ ] §11 Prior decisions: present (can be empty list).
- [ ] User explicitly approved the brief.

### After P1 Panorama → before P2 Producto

Panorama consistency:
- [ ] `README.md` mission is consistent with brief §2.
- [ ] `README.md` audiences table includes all from brief §3.
- [ ] `README.md` stack section consistent with brief §6.
- [ ] `BITACORA.md` exists with the 8-section template.
- [ ] `BITACORA.md` §0 dated today.
- [ ] Working language matches brief language.

### After P2 Producto → before P3 TechSpec

Product consistency:
- [ ] `product_detail.md` value statement non-empty.
- [ ] `product_detail.md` defines ≥1 MOD-NN.
- [ ] `product_detail.md` lists DOM-NN entities.
- [ ] `product_elements.yaml` mirrors structure of MD.
- [ ] `product_elements.yaml` valid YAML (parses without error).
- [ ] **NO SQL DDL** in `product_detail.md`. If found, regenerate.
- [ ] **NO JSON contracts** in `product_detail.md`. If found, regenerate.
- [ ] **NO sequence diagrams** for implementation. (High-level conceptual flow is OK.)
- [ ] Audiences in `product_detail.md` §5 consistent with Panorama and brief.

### After P3 TechSpec → before P4 Governance

SDD completeness:
- [ ] All 14 sections present.
- [ ] §1 Introduction declares purpose, scope, audiences, conventions.
- [ ] §2 Architecture has layered diagram (ASCII or markdown).
- [ ] §3 Data model has literal SQL DDL for each table.
- [ ] §3 includes ENUM listing.
- [ ] §4 API has at least one literal JSON contract example.
- [ ] §4 includes error catalog.
- [ ] §5 has at least one sequence diagram for the main pipeline.
- [ ] §7 Auth model declared.
- [ ] §8 has ≥5 ADRs.
- [ ] **Every ADR has a slug.** If missing, ask TechSpec to add.
- [ ] §10 Testing strategy non-empty.
- [ ] §11 Deployment has at least dev local.
- [ ] `plans/current.md` exists with numbered milestones.
- [ ] Each milestone has acceptance criteria.
- [ ] User explicitly approved.

ADR ↔ governance prep:
- [ ] All ADRs added to `pending_governance_mirrors` in state.

### After P4 Governance → before P5 Reference

Governance coverage:
- [ ] `SCHEMA.md` exists with adapted zones, layers, systems.
- [ ] `LOOP.md` exists.
- [ ] `CLAUDE.md` exists, project-specific.
- [ ] `STACK_NORMATIVO.md` exists, proportional to project context.
- [ ] All 5 `referencia/*.yaml` exist (zonas, capas, horizontes, proyectos, sistemas).
- [ ] **Every ADR has a mirror `dec-NNN-{slug}.md`**. Cross-check `pending_governance_mirrors` is empty.
- [ ] Each `dec-` has `espeja_adr: ADR-NNN` in front-matter.
- [ ] ≥3 policies derived from ADRs in `politicas/`.
- [ ] ≥1 norm in `normas/`.
- [ ] `semantica/glosario.yaml` exists.
- [ ] User explicitly approved.

### After P5 Reference

Reference coverage:
- [ ] `40_referencia/README.md` exists.
- [ ] Each sub-folder used has its own README.
- [ ] User-specified materials are classified (no orphans).
- [ ] `50_bitacora_y_revision/README.md` exists.
- [ ] `50_bitacora_y_revision/REVISION_GUIDE.md` adapted to the project.
- [ ] Materials moved (user ran `git mv` commands).
- [ ] User explicitly approved.

---

## Cross-layer consistency checks

These checks run **continuously** while phases progress, not only at boundaries.

### Audiences consistency

- Brief §3 audiences = Panorama audiences table = Product Detail §5 stakeholders.
- If divergence detected, escalate.

### Stack consistency

- Brief §6 stack = Panorama stack section = SDD §2.4 stack table = Governance `referencia/sistemas.yaml`.
- If divergence detected, escalate.

### Decision consistency

- Each ADR in SDD §8 = one `dec-` node with matching slug and `espeja_adr`.
- Each "decision already made" from brief §11 = one `dec-` node.

### Milestone consistency

- Milestones in `plans/current.md` = entries in `30_gobernanza/referencia/proyectos.yaml`.

### Policy ↔ ADR coverage

- Each ADR that has runtime / claude_code implications should be reflected in at least one `pol-` policy.
- If an ADR has no derived policy, flag as `policy_gap` — not blocking, but inform user.

### Norm ↔ regulation coverage

- Brief §5 regulations = norms in `30_gobernanza/normas/`.
- If a brief-mentioned regulation has no norm, flag.

---

## Auto-validation script (deferred)

In future versions, these checks could be a script. For now, the Orchestrator runs them as part of its responsibility.

---

## What to do when a check fails

| Failure | Action |
|---|---|
| Missing brief section | Send back to Discovery Agent in iteration mode. |
| Panorama missing audiences | Send back to Panorama Agent. |
| Product has SQL in it | Send back to Producto Agent: "Remove SQL; that's TechSpec's job." |
| SDD missing section | Send back to TechSpec Agent for that section. |
| ADR missing slug | TechSpec adds slug, no full re-run needed. |
| Governance missing dec- mirror | Add to TODO in Governance, ask agent to complete before approving phase. |
| Audiences inconsistency | Escalate to user to clarify. |
| Stack inconsistency | Escalate to user. |
| Policy gap | Inform user, ask if they want to add policy now or defer. |

---

## Validation log

The Orchestrator maintains a log:

```json
"validation_log": [
  {
    "from_phase": "P1",
    "to_phase": "P2",
    "checks_run": [list],
    "passed": [...],
    "failed": [...],
    "resolution": "user_approved_with_caveat | sent_back | escalated",
    "at": "ISO timestamp"
  }
]
```

Visible to the user upon request.

---

End of validation protocol.
