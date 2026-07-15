---
name: prd-decomposer
description: Breaks down a PRD into sequenced, dependency-ordered epics with prompts for GitHub SpecKit's /specify and /plan commands — for the solo or duo SpecKit workflow. Use this skill ONLY when the user is driving the build through SpecKit (i.e., they mention SpecKit, /speckit.specify, /speckit.plan, or spec-driven development). Trigger phrases include "prepare this for SpecKit", "generate SpecKit prompts", "decompose this into /specify and /plan prompts", "break this PRD into specs", or "go from PRD to SpecKit". Also trigger when the user shares a PRD and asks about build order, phasing, or parallel execution strategy for a SpecKit-driven build. Outputs lean specify prompts (what/why) and plan prompts (tech constraints) per epic. Do NOT use this skill when the target output is GitHub/Jira issues or a team backlog — decomposing into tracker work items is a different job. Also do NOT use for writing PRDs (use prd-builder) or reviewing PRDs (use prd-checker).
---

# PRD Decomposer

Break a PRD into epic-level work items, sequence them by dependency, and generate self-contained prompts for GitHub SpecKit.

## Scope

This skill is the final step in the PRD pipeline before engineering execution:

```
prd-builder → prd-checker → [prd-prototyper] → prd-decomposer → SpecKit
    ▲                          (optional)            │           OR hand to eng
    │                                                │
    └── feedback loops back to builder ──────────────┘
```

It produces the *inputs* to SpecKit — not the specs themselves. The SpecKit workflow:

1. **PRD Decomposer** → specify + plan prompts per epic
2. `/speckit.specify <prompt>` → agent generates the detailed spec
3. `/speckit.plan <plan prompt>` → agent generates the implementation plan
4. `/speckit.tasks` → agent breaks the plan into executable tasks
5. `/speckit.implement` → agent builds it

The decomposer owns step 1 only. Keep specify prompts lean — SpecKit expands them. Preserve technical decisions in plan prompts so they survive the handoff.

If a prototype was created via `prd-prototyper`, it may accompany the PRD. Use it as additional context for understanding the UX intent — it can clarify flows, screen layouts, and interaction patterns that the written PRD might describe abstractly. Reference specific prototype screens in specify prompts when it helps the agent understand what to build.

**Out of scope:** Writing PRDs from scratch (use `prd-builder`). Reviewing PRD quality (use `prd-checker`).

## The Decomposition Process

### 1. Ingest the PRD

Read the full PRD before touching anything. Build a mental model of the product's core purpose, the data model, integration points, feature dependencies, and what's shared infrastructure vs. feature-specific.

Bad decomposition almost always traces back to not understanding how the pieces connect. Resist the urge to start splitting immediately.

### 2. Clarify Ambiguities

After reading, scan for anything that would force guessing during decomposition. The PRD builder and checker should have caught most issues, but decomposition surfaces its own class of problems — things that are "clear enough" for a human reader but ambiguous when drawing epic boundaries.

**Clarify these — they change the decomposition:**

- **Shared-vs-separate ambiguity** — A feature described once that might need separate implementations in different contexts. *Example: "filtering" appears on the board and detail view — one shared component or two?*
- **Implicit features** — Requirements that imply functionality never explicitly described. *Example: "chat history persists across sessions" implies storage, session management, and a history UI — but the PRD only describes the chat.*
- **Boundary disputes** — Overlapping feature groups where ownership of shared behavior is unclear. *Example: does "initiative creation" own Confluence page auto-creation, or does the "Confluence integration" epic?*
- **Missing decisions** — TBDs that directly affect how you'd split or sequence epics.
- **Scale unknowns** — Features that could be trivial or massive depending on unstated assumptions. *Example: "drag-and-drop reordering" — simple list sort or full kanban with cross-column moves?*

**Skip these — they don't affect epic boundaries:**

- Items already flagged TBD in the PRD (the PM knows)
- Style/UX polish decisions
- Implementation choices (that's the agent's job)

**Batch your questions.** Present them all at once, grouped by which decomposition decision they affect. For each one, explain *why* it matters: "I need to know X because it determines whether this is one epic or two."

### 3. Identify Foundations

Before decomposing features, identify foundational layers that multiple features depend on. These become the first epics in the build sequence because everything else blocks on them.

Common foundations:

- **Data model / schema** — multiple features read/write the same entities
- **Auth / authorization** — anything requiring user identity
- **External integrations** — multiple features hit the same API (Jira, Confluence, etc.)
- **Core UI shell** — shared navigation, sidebar, layout framework
- **Shared components** — UI patterns reused across multiple screens

Foundations are usually *implied* by the PRD, not stated explicitly. A PRD might say "the board shows initiative cards" and "the detail view shows initiative data" without ever saying "we need a data model for initiatives" — but that's obviously the first thing to build.

**Checkpoint:** Present your inferred foundations as a quick list for confirmation before proceeding. Keep it fast — "Here's what I think the foundation layer looks like — confirm or correct before I decompose."

If any foundation assumption could cascade (e.g., inferring auth when an existing system might handle it, or assuming a tech stack that's still TBD), ask before proceeding.

### 4. Decompose into Epics

Work through the PRD's functional requirements and UX sections. The goal is epics at the right granularity — each one a coherent, independently buildable and testable unit of functionality that a developer or coding agent can implement end-to-end.

**Split when:**
- Distinct frontend/backend concerns that could be built independently
- Core behavior + advanced behavior that could ship separately
- Multiple integration points needing their own implementation
- Multiple screens/flows that are independently useful

**Keep together when:**
- Frontend and backend are tightly coupled and trivial together
- Splitting creates epics that can't be tested independently
- The feature is simple enough that splitting just adds overhead

For each epic, extract from the PRD: relevant functional requirements, UX details, data model needs, integration points, error states, acceptance criteria, and constraints.

**Read `references/ai-guardrails.md` before writing any epic.** AI agents miss implicit engineering requirements that human developers "just know." The guardrails checklist ensures each epic's prompts include domain-specific requirements (error handling, auth, logging, etc.) that the PRD may not spell out but the agent needs to build correctly.

### 5. Build the Dependency Graph

Map which epics depend on which:

- **Data** — "reads data another epic creates"
- **UI** — "lives inside a shell another epic builds"
- **Integration** — "uses a service client another epic sets up"
- **Logical** — "doesn't make sense without that feature existing"

**Phase the epics:**

| Phase | What goes here | Depends on |
|-------|---------------|------------|
| 0 — Foundations | Data model, auth, core integrations, UI shell | Nothing |
| 1 — Core | P0 features delivering primary value | Phase 0 |
| 2 — Supporting | P0 features that enhance core (AI, automation, advanced UX) | Phase 0-1 |
| 3 — Secondary | P1 features | Varies |
| 4 — Future | P2 features (post-MVP, included for completeness) | Varies |

Within each phase, order by dependencies first, then by value delivered.

**Identify parallel lanes.** Two epics can run in parallel if neither depends on the other, they don't modify the same files/tables in conflicting ways, and they can be tested independently. Group parallel-eligible epics into swim lanes.

**Call out sync points** — where an epic depends on multiple parallel lanes completing. These are bottlenecks where coordination matters.

**Flag conflict risks** — parallel epics that are technically independent but touch adjacent areas where merge conflicts or integration surprises are likely. Not blockers, but the agent should know.

### 6. Write SpecKit Prompts

For each epic, write prompts for SpecKit's two-step workflow.

**Specify prompt** → fed to `/speckit.specify`. Describes WHAT and WHY in 2-5 sentences. SpecKit expands this into full requirements, so keep it focused on product intent. Cover: what the feature does, who it's for and why, key behaviors/rules, and scope boundaries (what's NOT included).

**Example — good specify prompt:**
> Build the initiative board — a kanban-style home screen with Now, Next, and Later columns. PMs drag initiative cards horizontally to reclassify and vertically to reprioritize. Each card shows the initiative name, owner, lifecycle track progress, and AI-generated warning badges. This is the primary interface used during weekly PM prioritization meetings projected on a conference room screen. Does not include the initiative detail view or AI chat sidebar — those are separate features.

**Example — bad specify prompt (too much implementation):**
> Build a React component using react-beautiful-dnd with three column containers mapped to a PostgreSQL enum of NOW/NEXT/LATER. Each card component receives props for initiative name, owner avatar URL...

That's plan/implementation territory. The specify prompt should make SpecKit understand the *product*, not dictate the *code*.

**Plan prompt** (optional) → fed to `/speckit.plan`. Only include when the PRD specifies technical decisions the agent needs to respect — mandated tech stack, required integrations/APIs, infrastructure constraints, performance targets, security/auth requirements, or existing systems to integrate with.

Skip the plan prompt when the epic has no tech constraints from the PRD, the approach is TBD, or it's purely UI with no integration points. Let the agent decide.

**Example — good plan prompt:**
> PostgreSQL on AWS RDS for persistence. Integrate with Jira Cloud REST API across 7 projects for live epic and story point data. Jira API has rate limits — design a background sync strategy (every 5-15 minutes) rather than real-time polling. Expected scale is under 100 active initiatives.

### 7. Deliver

Read `references/output-template.md` for the exact output format and delivery options. Present the full decomposition as a structured document with the build sequence overview table, parallel execution map, and per-epic detail blocks.

Ask the user how they want it: markdown file (default for large PRDs), inline in chat (small PRDs), or individual files per epic.

If decomposition surfaced PRD issues — ambiguous requirements that couldn't be cleanly split, missing technical decisions that forced guessing on plan prompts, or scope gaps between epics — flag them. These should go back to **prd-builder** in update mode to fix the PRD before engineering acts on the decomposition.

## Edge Cases

**Incomplete PRD / TBDs:** Decompose what exists. Create placeholder epics for TBD sections with a note explaining what's blocking full specification. Write the best prompt you can with available context.

**No priority levels:** Ask the user to identify P0/P1/P2 before decomposing, or recommend priorities based on the dependency graph and get confirmation.

**Massive PRD (20+ epics):** Group the overview by phase. Suggest tackling one phase at a time rather than generating all prompts at once.

**Existing codebase references:** If the PRD mentions existing systems, APIs, or code, include those references in relevant epic prompts. The coding agent needs to know what already exists.

**User wants to skip phases:** That's fine — the sequence is a recommendation. Provide the requested epic's prompt with a note about which dependencies it assumes are in place.
