# Deliverable Spec · 00_panorama/

> Defines what the Panorama Agent must produce. Output files, structure, mandatory and optional sections.

---

## Mandatory outputs

### 1. `00_panorama/README.md`

**Purpose:** stable contextual entry point. The first thing a new collaborator reads.

**Length:** ~200-400 lines.

**Required sections:**
1. Mission (1-2 paragraphs).
2. Audiences table (audience type → needs → starts at).
3. Stack overview (high-level, with reference to SDD §2 for detail).
4. Roles and stakeholders.
5. Project status (pointer to `BITACORA.md`).
6. Regulation summary (pointer to `30_gobernanza/STACK_NORMATIVO.md`).
7. Existing materials (pointer to `40_referencia/`).

**Forbidden in this doc:**
- Implementation detail.
- Code snippets.
- Detailed regulation analysis.

### 2. `00_panorama/BITACORA.md`

**Purpose:** living state of the project, updated at milestone closures.

**Length:** ~150-300 lines (template).

**Required sections (8-section pattern):**
0. Last update + status.
1. How to resume work in a new session.
2. Status by milestone.
3. Pending external gates.
4. Recent decisions closed.
5. Next concrete deliverable.
6. Open risks (top 3).
7. Quick glossary.
8. How to update this BITACORA.

---

## Optional outputs

### 3. `00_panorama/stakeholders.md` (if RACI is large)

If brief §3 audiences + §8 roles produce >8 distinct roles, generate a separate RACI table here. Otherwise embed in `README.md`.

---

## Acceptance criteria

- [ ] README mission is **traceable to brief §2** (no invention).
- [ ] Audiences in README cover **all from brief §3**.
- [ ] BITACORA template skeleton present with 8 sections.
- [ ] Today's date in BITACORA §0.
- [ ] Cross-links to `../10_producto/`, `../20_especificacion_tecnica/`, `../30_gobernanza/` placed but not broken (links to files that don't exist yet are OK — they'll be created downstream).
- [ ] Language matches brief.
- [ ] No content that belongs in other layers (no SQL, no policy enforcement detail, no consultant intake).

---

## Common mistakes to avoid

- **Padding.** If brief is sparse, the panorama is sparse. Don't invent.
- **Stale stack references.** If brief says "Postgres + Next.js", README must say exactly that, not "modern database + frontend framework".
- **Mixing layers.** Mission belongs here, value proposition belongs in `10_producto/`. Don't duplicate.
