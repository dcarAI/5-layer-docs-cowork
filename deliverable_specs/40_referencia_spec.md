# Deliverable Spec · 40_referencia/

> Defines what the Reference Curator must produce.

---

## Mandatory outputs

### 1. `40_referencia/README.md`

Layer index. Explains sub-folder taxonomy. ~50-100 lines.

### 2. Sub-folder READMEs

For each sub-folder used (only those needed), a README explaining context.

---

## Sub-folder taxonomy

Create only the ones the project uses:

- `discovery/` — user research, interviews, journey maps. Often empty in MVP.
- `consultores/{nombre}/` — one per external methodological consultant.
- `demo/` — demo data plans, scripts, transcripts.
- `historico/` — material superseded but preserved (start_specs, old RFPs, prior implementations).

Do NOT create empty sub-folders unless explicitly mentioned in the brief.

---

## Within `consultores/{nombre}/`

Standard structure:

- `README.md` — who they are, what they contribute, current relationship status.
- `briefing.md` — 1-pager to share with the consultant (in their language).
- Material files: `{topic}_{YYYY_MM_DD}.{ext}`.
- `_archive/` — superseded deliveries.

---

## Within `demo/`

- `README.md`.
- `data_plan.md` — the plan for generating synthetic data.
- Subsequently (in implementation): `transcripts/`, `scripts/`, etc.

---

## Within `historico/`

- `README.md` — explanation of preserved material.
- Subdirectories per source: `start_specs/`, `prior_rfps/`, etc.

---

## Naming conventions

| Material type | Pattern |
|---|---|
| Briefing for/from consultant | `briefing.md` |
| Consultant deliverable file | `{topic}_{YYYY_MM_DD}.{ext}` (e.g., `matrix_2026_05_26.xlsx`) |
| Demo data plan | `data_plan.md` |
| Historical spec | `{NN}_{name}.{ext}` keeping the original name |

**Move don't copy.** When the user moves materials in their working directory, use `git mv` to preserve history.

---

## Acceptance criteria

- [ ] `README.md` of layer exists.
- [ ] Each used sub-folder has README.
- [ ] Materials inventoried per brief §7.
- [ ] No orphan materials (not in any sub-folder).
- [ ] Naming consistent.
- [ ] User confirmed where each material lives.

---

## Common mistakes to avoid

- **Creating sub-folders without need.** Discovery folder empty in MVP is fine; just don't create unrelated sub-folders.
- **Generating substantive content.** This layer organizes; it doesn't produce product or technical content.
- **Modifying source materials.** Excel/PDF stay as user provided; only the surrounding context (README) is generated.
