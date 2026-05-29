# Intake Protocol — structure of the discovery conversation

> Used by the **Discovery Agent** in Phase 0. Defines question sequencing, follow-ups, and the brief format.

---

## Conversation phases

### Phase A · Opening (1-2 turns)

- Agent introduces itself briefly.
- Asks for project name.
- Sets expectation: "I'll ask you 15-20 questions one at a time. We'll spend ~30 min. At the end, you'll have a brief that downstream agents use to generate all your project's documentation."

### Phase B · Identity & mission (3-4 questions)

- Project name.
- One-line elevator pitch.
- What problem it solves that wasn't being solved before.
- Owning organization.

**Follow-up trigger:** if mission is vague ("we want to improve X"), drill: "What specifically about X is broken today?"

### Phase C · Audiences (2-3 questions)

- Primary user type with role and context.
- Secondary stakeholders (CHRO, CFO, buyer, etc.).
- External methodological consultants if any.

**Follow-up trigger:** if user says "everyone", push back: "Be specific about the most important user. The framework adapts to who reads each layer."

### Phase D · Phase of the project (2-3 questions)

- Where the project stands today (idea / prototype / MVP / mid-implementation / mature).
- Major external gates in the next 3-6 months (audit, certification, demo to client).
- Approximate timeline goals.

### Phase E · Domain & regulation (3-4 questions)

- Industry / sector.
- Geographic deployment (EU? US? both? other?).
- Does the AI Act apply (are they a deployer or provider of an AI system)?
- Is personal data processed under GDPR / HIPAA / CCPA?

**Follow-up triggers:**
- EU + AI → mention AI Act Art. 50 transparency obligation from Dec 2026.
- EU + PII → mention GDPR Art. 6 base legal and DPA requirements.
- High-risk sectors (medical, finance, hiring) → flag for AI Act high-risk classification consideration.

### Phase F · Technology stack (4-6 questions)

- Backend language/framework (Python? Node? Go? other?).
- Frontend (Next.js? Vue? Svelte? native?).
- Database(s) (Postgres? MySQL? MongoDB? specialized?).
- Cloud / hosting target (AWS? GCP? Azure? self-hosted?).
- Auth provider (Clerk? Auth0? GCP Identity Platform? custom? none?).
- AI providers: LLM, embeddings, vision, audio.

**Follow-up triggers:**
- If LLM mentioned → ask which (Anthropic? OpenAI? Google?) and which model tier.
- If vector DB needed → ask if pgvector or dedicated (Pinecone? Weaviate?).

### Phase G · Existing materials (2-3 questions)

- Excel, docs, decks from consultants.
- Prior implementation to reference.
- Demo data plans.

**Follow-up trigger:** if user has consultant material in a foreign language, note it for translation/handling in Phase 5 Reference.

### Phase H · Roles (2 questions)

- Technical lead (likely the user).
- Methodology owner / DPO / compliance lead if separate from tech.

### Phase I · Constraints (2-3 questions)

- Budget headlines (if comfortable to share).
- Hard deadlines.
- Explicit out-of-scope items.

### Phase J · Prior decisions already made (1 question)

- Decisions already cared about (will be `dec-` nodes).

Examples to prompt: "we'll use Postgres", "we deploy in EU only", "we don't store raw transcripts longer than X".

### Phase K · Open questions for later (1 question)

- Things the user could not answer right now.

### Phase L · Brief synthesis (1 turn)

Agent generates the full brief draft and presents it.

### Phase M · User validation

User reads, requests changes, or approves.

Agent regenerates if changes requested.

### Phase N · Closure

User says "approved" or equivalent. Agent ends with the closing message defined in `agents/discovery_agent.md`.

---

## Question sequencing rules

- **One question per turn.** Never bundle.
- **Group thematically.** Don't jump from tech stack to budget mid-conversation.
- **Restate briefly.** After each answer, summarize what you heard before next question.
- **Park what's unknown.** If user says "I don't know", note as `unknown` and continue. Drill only if it blocks the rest of the conversation.

---

## Follow-up patterns

| User says... | Agent follows up with... |
|---|---|
| "It's complicated" | "Let's break it down. First, what's the simplest version?" |
| "Everyone is the user" | "Let's identify the most important one — who pays or sponsors?" |
| "I don't know yet" | "Mark as deferred. Want to come back to this at the end?" |
| "Skip discovery, generate the SDD" | "Trust the process. The brief is what makes downstream agents work. ~20 more minutes." |
| Gives a 5-paragraph answer | "Got it. Let me summarize: {1-sentence summary}. Move on?" |

---

## Brief format (output)

See `agents/discovery_agent.md` — the brief.md format is defined there literally.

---

## Anti-patterns to avoid

- **Don't generate any layer's content yet.** Discovery is exclusively conversation + brief.md.
- **Don't bombard.** 20 questions, not 50.
- **Don't lecture.** No "let me explain the framework" speeches.
- **Don't translate the user's words.** Match their language and style.
- **Don't infer.** If you don't know, ask.

---

## Validation at the end of P0

Before declaring P0 complete:

- [ ] Brief has all 11 sections.
- [ ] §1 Identity is non-empty.
- [ ] §2 Mission has at least 1 paragraph.
- [ ] §3 Audiences has at least 1 primary user.
- [ ] §4 Phase declared.
- [ ] §5 Regulation explicit (even if "none applicable").
- [ ] §6 Tech stack has at minimum: backend, frontend, db.
- [ ] §7 Existing materials inventoried.
- [ ] §11 Decisions already made (can be empty list, but section present).
- [ ] User explicitly approved.

If any check fails, ask the user and re-run that section's questions.

---

End of intake protocol.
