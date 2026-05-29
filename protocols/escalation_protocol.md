# Escalation Protocol — when and how to stop and ask the user

> Used by **all agents** when they detect a situation that requires user decision. Defines escalation triggers and format.

---

## When to escalate

### Mandatory escalation

1. **Contradiction** between current input and prior phase output.
2. **Missing information** that cannot be reasonably inferred.
3. **Architectural decision with no clear winner** (multiple valid options).
4. **Regulation applicability uncertain** (e.g., "is this 'high-risk AI' under AI Act?").
5. **Domain expertise gap** ("I'm not sure what this consultant Excel represents").
6. **Out-of-scope request** ("can you write code?" "can you negotiate?").
7. **Validation failure** that blocks the phase.

### Optional escalation (recommended)

1. **Trade-off visible**: agent could choose, but the user has stake.
2. **Stylistic decision**: e.g., naming.
3. **Optional output**: e.g., should we generate `revision_review.html`?

---

## Escalation format

When an agent escalates, it uses this structured format:

```markdown
## 🛑 ESCALATION

**From:** {Agent name}
**During:** Phase {N} — {phase name}
**Severity:** {blocking | informative | recommendation}

### Context

{1-2 sentences explaining where in the work this came up.}

### Question for the user

{Single clear question.}

### Options

1. **Option A.** {Description.} Trade-off: {what gained, what lost}.
2. **Option B.** {Description.} Trade-off: {...}.
3. **Other.** {Free-text if user wants something else.}

### Recommendation (if any)

{Agent's recommendation with brief justification. Mark explicitly as "Recommended" so the user knows.}

### Impact on the project

- If user chooses A: {downstream impact}.
- If user chooses B: {downstream impact}.

### Pending

Work on Phase {N} is paused until user responds.
```

---

## After user responds

1. Agent acknowledges decision.
2. Updates state JSON with the decision (if it's a `dec-` candidate, also note for Governance Agent).
3. Resumes work on Phase {N}.
4. Continues until next checkpoint or end of phase.

---

## Escalation severity

| Severity | Behavior |
|---|---|
| **Blocking** | Phase cannot proceed. Agent stops, waits for user. |
| **Informative** | Phase proceeds with default; user is informed and can override later. |
| **Recommendation** | Phase proceeds with default; agent recommends user review at next milestone. |

---

## Escalation registry

The Orchestrator maintains all escalations in state:

```json
"escalations": [
  {
    "id": "E-001",
    "from_agent": "techspec",
    "during_phase": "P3",
    "severity": "blocking",
    "question": "...",
    "options": [...],
    "user_decision": "A",
    "decision_at": "ISO timestamp"
  }
]
```

User can request: "list open escalations" or "list all decisions made via escalation".

---

## Anti-patterns

- **Silently choosing.** If the agent doesn't know, it must ask. Silent defaults erode trust.
- **Too many escalations.** If agents are escalating every other turn, the Discovery brief is too weak. Recommend re-running parts of P0.
- **Open-ended questions.** Always offer 2-3 options + "Other". Don't ask "what should we do?" without scaffolding.
- **Burying the question.** The 🛑 emoji and the format make escalations visually distinct. Don't hide them in long responses.

---

## Examples

### Example 1 — blocking

```
## 🛑 ESCALATION

**From:** TechSpec Agent
**During:** Phase 3 — Technical Specification
**Severity:** blocking

### Context

Generating §3 Data Model. The `Behavior` entity has a KPI pair (anchor + blocker). I need to decide the data type for `kpi_valence`.

### Question

What Postgres type should `kpi_valence` be?

### Options

1. **ENUM (recommended).** Native Postgres ENUM `('anchor', 'blocker')`. Pros: type safety, performance, documentation. Cons: schema migration to add values requires ALTER TYPE.
2. **TEXT with CHECK constraint.** Flexible. Cons: no native enforcement of values in ORM tools.

### Recommendation

ENUM. It matches the project's philosophy of strong typing and aligns with other ENUMs already defined in §3.3.

### Pending

§3 generation paused.
```

### Example 2 — informative

```
## 🛑 ESCALATION

**From:** Reference Curator
**During:** Phase 5 — Reference
**Severity:** informative

### Context

You mentioned an Excel file in your brief that was an "old version of the master catalog" from a consultant. I want to confirm where to file it.

### Question

Where should `catalog_old_v0_3.xlsx` live?

### Options

1. **`40_referencia/consultores/{consultant}/_archive/`.** Treated as superseded but preserved.
2. **`40_referencia/historico/`.** Treated as project-wide history.

### Recommendation

Option 1 if you'll keep working with this consultant; option 2 if the consultant relationship is ending.
```

---

End of escalation protocol.
