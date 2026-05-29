# Iteration Protocol — managing loops backward

> Used by the **Orchestrator** when user signals iteration ("go back to Phase N") or when an agent detects a gap that requires returning to a prior phase.

---

## When iteration happens

Common triggers:

- User mid-Phase 3 realizes a domain entity wasn't captured in Phase 2 → iterate to P2.
- User reads Panorama and wants to revise the mission → iterate to P1.
- Governance Agent detects an ADR with no corresponding `dec-` because the SDD ADR was actually two decisions → iterate to P3.
- New external constraint (e.g., regulator audit announced) → re-run STACK_NORMATIVO in P4.
- Methodology owner provides new material → update P4 references, possibly P2.

---

## Iteration ≠ restart

Iteration is **targeted update**, not full restart. The agent of the target phase reads existing outputs and asks what changed.

---

## Process

### Step 1 · User signals iteration

User says: "Necesito volver a Fase {N}" or "Cambio en {topic}".

### Step 2 · Orchestrator pauses current

- Marks current phase as `paused`.
- Saves state snapshot.

### Step 3 · Orchestrator activates target agent in iteration mode

Activation message:

```
You are entering ITERATION MODE on Phase {N}.

Previous output of this phase was approved on {date}.
User now needs to update because: {reason described by user}.

Read existing output: {path/to/file}.

DO NOT start from scratch. Instead:
1. Acknowledge the existing output.
2. Ask the user: "What specifically changed?"
3. Update only the affected sections.
4. Re-validate consistency.
5. Present the updated output for approval.
```

### Step 4 · Target agent runs iteration

Agent:

1. Reads existing output.
2. Asks user: "What specifically changed?"
3. User explains.
4. Agent updates the relevant sections (not the whole document).
5. Presents diff or full updated doc.
6. User approves.

### Step 5 · Downstream consistency check

Orchestrator must verify that downstream phases still hold.

| Phase iterated | Downstream phases to check |
|---|---|
| P0 Discovery | P1, P2, P3, P4, P5 — full re-validation usually needed |
| P1 Panorama | P2 audiences, stack — likely affected |
| P2 Producto | P3 entities + modules in SDD — usually affected |
| P3 TechSpec | P4 dec- mirrors — definitely affected if new ADR |
| P4 Governance | P5 if new norms — usually OK |
| P5 Reference | (terminal, no downstream) |

For each downstream phase that needs check:

- Re-read its output.
- Compare against new upstream output.
- If still consistent → mark `consistent_after_iteration`.
- If not consistent → mark `needs_review`, propose re-iteration of that phase.

### Step 6 · Inform user

Orchestrator reports:

```
ITERATION COMPLETED · Phase {N} updated.

DOWNSTREAM IMPACT:

✓ Phase {M}: consistent, no changes needed.
✗ Phase {M+1}: needs review. Reason: {specific inconsistency}. Recommend re-iterating Phase {M+1}.

Next action:
- Approve and move on, OR
- Re-iterate Phase {M+1} (recommended).
```

User decides.

---

## Iteration limit

If the same phase is iterated >3 times in a project, escalate to user:

> "We've iterated on Phase {N} 3 times. Something fundamental may be unresolved. Pause and reflect: is the brief still accurate? Should we run a follow-up Discovery on a specific topic?"

---

## Iteration vs new phase

Sometimes the user thinks they want to iterate but actually need a new phase:

- "I want to add observability" → not iteration; consider it as a new horizontal layer or an addition to TechSpec §9 NFRs.
- "I want to add a new module" → iteration of P2 + P3 + P4.

The Orchestrator clarifies before activating.

---

## Iteration log

Maintain in state:

```json
"iteration_log": [
  {
    "phase": "P2",
    "triggered_by": "user",
    "reason": "Forgot DOM-13 entity",
    "downstream_impact": ["P3 needs SDD §3 update"],
    "resolved_at": "ISO timestamp"
  }
]
```

---

End of iteration protocol.
