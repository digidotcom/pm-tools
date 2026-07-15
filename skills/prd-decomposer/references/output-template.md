# Output Template

Use this exact structure when delivering the epic decomposition. Adapt section counts to the PRD — not every decomposition will have all phases.

## Template

```markdown
# [Product Name] — Epic Decomposition

## Build Sequence Overview

| Phase | Lane | Epic | Priority | Dependencies | Summary |
|-------|------|------|----------|-------------|---------|
| 0     | —    | [name] | P0 | None | [one-liner] |
| 1     | A    | [name] | P0 | [epic names] | [one-liner] |
| 1     | B    | [name] | P0 | [epic names] | [one-liner] |
| 1     | sync | [name] | P0 | [epic names] | [one-liner] |
| ...   | ...  | ...  | ...| ... | ... |

## Parallel Execution Map

Phase 0: [Foundation epics — typically sequential or all-parallel if independent]
Phase 1:
  Lane A: Epic X → Epic Y
  Lane B: Epic Z
  ⚠️ Conflict risk: [note if applicable]
  ── sync point ──
  Epic W (requires Lane A + B complete)
Phase 2:
  ...

## Phase 0: Foundations

### Epic: [Name]
**Dependencies:** None
**Priority:** P0
**Parallel:** [Can run alongside X, Y / Must complete before Z]

**Specify Prompt:**
> [Concise what/why description for /speckit.specify]

**Plan Prompt:**
> [Tech constraints from the PRD for /speckit.plan — omit this section entirely if no constraints apply]

**AI Guardrails:**
> [Domain-specific implicit requirements from the guardrails checklist — omit if none apply to this epic]

---

### Epic: [Name]
...

## Phase 1: Core Features

### Epic: [Name]
**Dependencies:** [List epic names this depends on]
**Priority:** P0
**Parallel:** [Can run alongside X / Blocks Y]

**Specify Prompt:**
> [...]

**Plan Prompt:**
> [... or omit if no tech constraints]

**AI Guardrails:**
> [... or omit if none apply]

---

## Phase 2: Supporting Features
...

## Phase 3: Secondary Features
...

## Phase 4: Future
...
```

## Delivery Options

Ask the user which format they prefer:

- **Markdown file** — the full decomposition as a downloadable `.md` file. Default for large PRDs or anything with 8+ epics.
- **Inline in chat** — good for smaller PRDs with fewer epics where the user wants to iterate quickly.
- **Individual files** — one `.md` file per epic, each containing the specify prompt and plan prompt. Useful when the user wants to feed them to SpecKit one at a time or hand different epics to different agents.

## Formatting Notes

- The Build Sequence Overview table gives the PM a quick scannable view of the entire plan. Keep summaries to one short sentence.
- The Parallel Execution Map is the visual representation of what can happen concurrently. Use `→` for sequential dependencies within a lane, `── sync point ──` where lanes converge, and `⚠️` for conflict risks.
- Each epic's detail block should be self-contained — a PM should be able to copy-paste one epic block and hand it to an agent without needing the rest of the document for context.
- Omit sections that don't apply (Plan Prompt, AI Guardrails) rather than including them with "N/A."
