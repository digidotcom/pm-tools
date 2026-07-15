---
name: prd-builder
description: Builds comprehensive PRDs from whatever context the PM provides — prompts, uploaded docs, rough notes, Slack threads, meeting transcripts, or partial specs. Ingests all provided material first, maps it to a proven PRD template, then interviews only for gaps. Also handles updating existing PRDs when scope changes or stakeholder feedback comes in. Use this skill when the user wants to create a PRD, write product requirements, draft a product spec, build a requirements document, update an existing PRD, or formalize a feature idea. Trigger when the user says things like "let's write a PRD", "build a PRD from this", "I need a spec for", "help me define requirements", "new feature spec", "product requirements for", "start a PRD", "turn this into a PRD", "update this PRD", "stakeholder wants changes to the spec", or shares feature context and wants it formalized. Do NOT use for reviewing existing PRDs (use prd-checker) or breaking PRDs into epics (use prd-decomposer).
---

# PRD Builder

Build comprehensive PRDs from whatever the PM brings — a detailed prompt, uploaded documents, rough notes, meeting transcripts, or just a verbal brain dump. The PM's input is the primary source material. Interview questions fill gaps, not drive the process.

## Scope and Pipeline Context

This skill produces a complete PRD document following the template in `references/prd-template.md`. It's the first step — and the single source of truth — in the PRD pipeline:

```
prd-builder → prd-checker → [prd-prototyper] → prd-decomposer → SpecKit
    ▲              │              │                   OR hand to eng
    │              │              │
    └──────────────┴──────────────┴── feedback loops back to builder
```

1. **PRD Builder** (this skill) → creates and maintains the PRD
2. **PRD Checker** (`prd-checker`) → reviews for completeness, consistency, and agent-readiness
3. **PRD Prototyper** (`prd-prototyper`) → *(optional)* builds a clickable prototype for stakeholder validation
4. **PRD Decomposer** (`prd-decomposer`) → breaks into sequenced epics with SpecKit prompts — OR hand the PRD (and prototype) to engineering directly

Every downstream step can generate feedback that changes the PRD. When that happens, the loop comes back here — the builder in update mode. The checker finds issues? Update the PRD, re-check. Prototype reveals a bad flow? Update the PRD, re-prototype. Engineering pushes back on feasibility? Update the PRD. The builder is always the place where the PRD gets fixed.

The PRD this skill produces feeds directly into the checker and everything downstream. That means:

- **Functional requirements need clear priority levels (P0/P1/P2)** — the decomposer uses these for phasing
- **Feature groups should map to independently buildable units** — the decomposer splits along these boundaries
- **Technical decisions should be explicit, not implied** — the decomposer preserves them in SpecKit plan prompts
- **Scope boundaries (non-goals) need to be crisp** — the checker flags vague scope, and the decomposer needs to know where features end

When the PRD is complete, suggest the PM run it through `prd-checker`.

## Two Modes: New Build vs. Update

### New Build
The PM wants to create a PRD from scratch (or from source material). Follow the full build process below.

### Update
The PM has an existing PRD and needs to modify it. Updates typically come from one of three places in the pipeline:

- **Checker feedback** — the PRD failed quality checks or the agent-ready gate. The checker report identifies specific issues to fix.
- **Prototype feedback** — stakeholders reacted to the prototype and want changes to flows, scope, or priorities.
- **Engineering feedback** — the dev team pushed back on feasibility, surfaced technical constraints, or questioned scope.

Regardless of the source, the update process is the same:

1. **Ingest the existing PRD and the change request** — understand both the current state and what needs to change.
2. **Assess impact** — does this change affect just one section, or does it cascade? A scope change to Functional Requirements might invalidate User Stories, shift priorities, change Success Metrics, or alter the Narrative.
3. **Present the impact** — "This change touches Requirements and UX. It also means Success Metric #3 no longer applies. Here's what I'd update." Let the PM confirm before editing.
4. **Make surgical edits** — edit only the affected sections in place. Don't rewrite sections that aren't impacted.
5. **Re-check consistency** — after edits, verify cross-section alignment (goals ↔ metrics, stories ↔ requirements, scope ↔ non-goals). Flag anything that's now misaligned.
6. **Suggest re-running downstream** — after updates, the PM should re-run the checker (and re-prototype if the UX changed) before proceeding.

## The Build Process (New Build)

### 1. Ingest Everything

Read the PM's prompt and all provided files before doing anything else. Source material could be uploaded documents, rough notes, Slack/Teams exports, meeting transcripts, existing partial specs, competitive analysis, customer feedback, or mockup descriptions.

Absorb it all. Don't summarize it back.

### 2. Determine Project Type

Figure out whether this is internal or external from context. If ambiguous, ask — this determines how Business Goals are framed:

- **External (customer-facing):** Full business case — revenue, retention, market position, OKR alignment.
- **Internal:** Simplified — what internal problem it solves, who benefits, what efficiency or capability it unlocks. Skip revenue/market impact.

### 3. Map to Template

Map the provided content to PRD template sections (see `references/prd-template.md`). Assess each section as one of:

| Status | Meaning | Action |
|--------|---------|--------|
| **Covered** | PM's input clearly addresses this section | Draft it — no questions needed |
| **Partial** | Some content exists but incomplete or vague | Draft what's there, ask about the gaps |
| **Empty** | Nothing in the provided material | Ask targeted questions from `references/question-bank.md` |

### 4. Gap Analysis

Present the PM with a structured status report using this exact format:

> **Coverage Report**
>
> | Section | Status | Notes |
> |---------|--------|-------|
> | Goals | ✅ Covered | Business + user goals clear from input |
> | User Stories | 🟡 Partial | Have personas, need their workflows |
> | Functional Requirements | ✅ Covered | Detailed in uploaded doc |
> | User Experience | 🟡 Partial | Entry point clear, core flow needs detail |
> | Technical Considerations | ✅ Covered | Tech stack specified |
> | Success Metrics | ❌ Empty | Nothing in source material |
> | Narrative | ❌ Empty | Will draft after other sections |
> | tl;dr | ⏳ Last | Written after everything else |
>
> I'll draft the Covered sections now, then ask about the gaps. Sound good?

Wait for confirmation before proceeding. The PM might say "actually, skip Success Metrics for now" or "I have more context for UX, let me paste it."

### 5. Draft First, Ask Second

This is the core principle: **if you have 60% or more of a section's content, draft it and confirm — don't interview.**

**For Covered sections:** Draft the full section from the PM's input. Present it for confirmation. Move on.

**For Partial sections:** Draft what you can from the input, then ask only about what's missing. Frame gaps as specific questions, not open-ended prompts. "Your input describes the filtering UI but doesn't mention what happens when filters return no results — should it show an empty state, a suggestion, or reset the filters?" is better than "Tell me about error states."

**For Empty sections:** Fall back to targeted questions from `references/question-bank.md`. Ask 3-5 at a time, synthesize, confirm, move on. Even here, try to infer from context — if the PM described a feature in detail elsewhere, you can probably draft User Stories or Success Metrics from that without asking.

**The draft-first rule matters because** PMs are better at reacting to a draft than answering abstract questions. A draft gives them something to push back on, refine, or approve. Questions give them homework.

### 6. Build the PRD

Create the PRD as a **markdown file** (`.md`) — this is the default because it's what the checker and decomposer consume downstream, and it's readable everywhere.

If the PM needs a Word doc for stakeholders or a review process, offer to create a `.docx` version after the markdown is finalized. Build in markdown first, convert second.

Write the document section by section:

- Start with the sections that have the most complete input — build momentum.
- Create the file after writing the first section, then update it incrementally with targeted edits.
- The **tl;dr** is always written last — it's a synthesis of everything else.
- For skipped sections, include a `[TBD]` placeholder with a note about what's needed to complete it.

### 7. Review and Iterate

Present the full PRD and do a self-check:

- Consistency: goals ↔ metrics, user stories ↔ requirements, scope ↔ non-goals
- Gaps or contradictions
- Vague requirements that need specificity for the decomposer
- Priority levels (P0/P1/P2) on all functional requirements
- Feature groups that map to buildable units

Ask the PM to review. Iterate with targeted edits — change only what the feedback touches.

### 8. Handoff

When the PM is satisfied, suggest next steps:

> PRD looks solid. Next step is **prd-checker** to validate completeness and agent-readiness. After that:
> - *(Optional)* **prd-prototyper** — build a clickable prototype to validate with stakeholders. The prototype travels with the PRD so engineering can see it too.
> - Then **prd-decomposer** to break it into sequenced epics with SpecKit prompts — or hand the PRD and prototype to engineering directly.

Don't push — some PMs will want to socialize the PRD with stakeholders before moving forward. The suggestion is enough.

## Best Practices

**Draft-first is the default.** Interview is the fallback. If you're asking more than 10 questions total across the entire build, you're probably under-leveraging the PM's input.

**Pacing:** When you do ask questions, 3-5 at a time. Accept shorthand. Summarize before moving sections.

**Probing:** Vague answers → ask for examples. Unclear scope → ask "what's NOT included?" Unclear priorities → "if you could only ship one thing?"

**Flexibility:** Skip sections on request (mark TBD). Capture useful tangents in the right section. If the PM pastes additional context mid-build, absorb it and update.

**Contradictions in source material:** Flag explicitly. Don't silently pick a version — ask which is current.

## Reference Files

- `references/question-bank.md` — Targeted questions for each PRD section, used to fill gaps
- `references/prd-template.md` — The complete PRD markdown template
