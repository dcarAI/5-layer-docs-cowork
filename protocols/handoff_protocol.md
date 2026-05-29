# Handoff Protocol — how agents transfer work

> Used by **all agents** at phase boundaries. Defines the format of phase-closing messages and how the Orchestrator routes to the next agent.

---

## Closing message format

When an agent finishes its phase, it MUST produce a structured closing message:

```markdown
## Phase {N} closed — {Phase name}

### Outputs produced

- `{path}/{file1}` — {1-line description}
- `{path}/{file2}` — {1-line description}

### Decisions registered (if any)

- {decision 1 — typically only relevant for TechSpec generating ADRs}

### Open questions raised

- Q-{NNN}: {question text} → escalated to {user | other agent}

### Pending downstream work

- {Item that the next agent must process, e.g., "ADR-001 to ADR-013 must be mirrored as `dec-` nodes by Governance Agent"}

### Signal to Orchestrator

> Phase {N} complete. Next phase: {N+1}. Activate {next_agent_name}.
```

---

## Orchestrator validation upon receipt

When the Orchestrator receives a closing message, it MUST:

1. **Verify outputs exist** in the project's `docs/` (or the user-confirmed location).
2. **Run validation checks** for the closing phase (per `validation_protocol.md`).
3. **Update state JSON**:
   ```json
   {
     "completed_phases": [..., "P{N}"],
     "outputs": { ..., "{path}/{file}": "approved_YYYY-MM-DD" },
     "pending_governance_mirrors": [..., {new ADRs from TechSpec}],
     "open_questions": [..., {new questions}]
   }
   ```
4. **Report to user**: brief summary of what was completed.
5. **Wait for user signal** to advance ("pasa a Fase N+1") before activating next agent.

---

## When user signals advance

User says (or equivalent): "Pasa a Fase {N+1}, activa {agent_name}".

Orchestrator:

1. Reads `pending_governance_mirrors` and `open_questions` (still relevant for the next phase).
2. Constructs **handoff package** for next agent:
   ```
   Activating {next_agent_name}.

   ## Context

   - Brief: `intake/brief.md` (P0, approved {date}).
   - Panorama: `docs/00_panorama/README.md` (P1, approved {date}).
   - {... other prior outputs}

   ## State

   {Full state JSON}

   ## Specific instructions for this phase

   {Phase-specific notes, e.g., "Use the ADR list from §8 of sdd.md as input for dec- mirroring."}

   ## Open questions to resolve in this phase

   - Q-{NNN}: {if any are relevant}
   ```
3. Activates the next agent with this package as initial context.

---

## Iteration handoff

If user wants to revisit a phase (`"vuelve a Fase 2"`):

1. Orchestrator marks current phase as `paused`.
2. Reads back state to before P_target was completed.
3. Reactivates P_target's agent with: "Iteration mode. Existing output was approved on {date}. User wants to update. Read existing output first, then ask what changed."
4. After P_target is updated, Orchestrator **checks downstream consistency**:
   - If P3 outputs depend on P2 (always yes), ask P3 agent to re-validate against new P2.
   - If P3 still consistent → no rework needed.
   - If P3 no longer consistent → flag as `needs_review`, propose re-iteration.

---

## Multi-agent collaboration handoff

In rare cases, two agents collaborate (e.g., TechSpec and Governance during ADR mirroring).

Pattern:

1. Agent A produces output.
2. Agent A's closing message includes `Pending downstream work: {specific items for Agent B}`.
3. Orchestrator routes to Agent B with `Pending work from Agent A: {items}`.
4. Agent B processes items + own scope.

Example:

- TechSpec finishes P3 with 13 ADRs.
- Closing message: "Pending downstream work: 13 ADRs need `dec-` mirrors in Governance."
- User → Orchestrator → "Pasa a Fase 4".
- Orchestrator → Governance Agent: "Process 13 ADRs from §8 of sdd.md as `dec-` mirrors, then generate schema + policies."
- Governance generates everything in one phase.

---

## Anti-patterns

- **Skipping the closing message.** Each phase must close formally; otherwise the Orchestrator can't validate.
- **Auto-advancing without user signal.** The user is always the final gate.
- **Multiple agents active at once.** Only one substantive agent at a time; Orchestrator coordinates.
- **Skipping validation checks.** Even if the user is eager, the checks prevent downstream cascading errors.

---

End of handoff protocol.
