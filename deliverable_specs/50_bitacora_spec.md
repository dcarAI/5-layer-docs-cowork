# Deliverable Spec · 50_bitacora_y_revision/

> Defines what the Reference Curator (or dedicated agent if separated in future) produces for the meta layer.

---

## Mandatory outputs

### 1. `50_bitacora_y_revision/README.md`

Layer index. ~30-80 lines.

**Required:**
- What this layer is for (review and traceability — not the BITACORA itself, which lives in `00_panorama/BITACORA.md`).
- When to use it.
- Pointer to `REVISION_GUIDE.md`.

### 2. `50_bitacora_y_revision/REVISION_GUIDE.md`

Narrative guide for critical review of project documentation.

**Length:** typically 150-300 lines.

**Required sections:**
1. The problem this guide solves.
2. **Four itineraries** by review objective (adapt to project):
   - A: Pre-implementation technical ratification.
   - B: Governance coverage validation.
   - C: Material prep for external party (consultant, customer).
   - D: Regulator audit preparation.
3. For each itinerary: ordered reading list, what to look for, time estimate.
4. Table: canon vs derivative documents.
5. Practical rules to avoid wasting time.
6. What to do with findings (categories of action).
7. Pointer to interactive HTML if generated.

---

## Optional outputs

### 3. `50_bitacora_y_revision/revision_review.html`

Standalone HTML checklist with:
- List of all project documents (canonical + derivative).
- Filter by capa, priority, status.
- Toggle pendiente/revisado/con-notas per document.
- Notes per document + global notes.
- Persistence in localStorage.
- Export/import JSON.

Generate **only if the project has >15 documents** and the user wants visual tracking.

### 4. `50_bitacora_y_revision/revisiones/{date}.json`

Exports of the HTML state (snapshot of a completed review session). Generate per user request after a review session.

### 5. `50_bitacora_y_revision/changelogs/`

Aggregated changelog of the project. Optional, generated periodically.

---

## What's NOT here

The **BITACORA** (living journal) lives in `00_panorama/BITACORA.md`, not here. This layer is for **review tooling**, not project state.

If unclear: `00_panorama/BITACORA.md` is the project's diary; `50_bitacora_y_revision/` is the tool for reviewing all documentation against goals.

---

## Acceptance criteria

- [ ] `README.md` exists and explains the distinction with `00_panorama/BITACORA.md`.
- [ ] `REVISION_GUIDE.md` has ≥3 itineraries adapted to project audience.
- [ ] Each itinerary has time estimate and "what to look for".
- [ ] Canon vs derivative table covers all project docs.
- [ ] User approved.

---

## Common mistakes to avoid

- **Duplicating BITACORA here.** It belongs in 00_panorama/.
- **Including substantive content review** (that's the reviewer's job, not the doc's).
- **HTML for small projects.** If <15 docs, the markdown guide alone is sufficient.
- **Over-detailed itineraries.** Each itinerary is a short ordered list + key questions. Don't expand into chapters.
