---
name: prd-prototyper
description: Generates interactive UI prototypes from PRDs — either as a React artifact directly in Claude.ai or as a structured prompt for a coding agent (Claude Code, etc.) to build a fuller app. Use this skill when the user wants to prototype a feature from a PRD, visualize a UX flow, create a clickable demo, build a proof of concept, or show stakeholders what something will look and feel like before engineering commits. Trigger when the user says things like "prototype this PRD", "build a demo from this spec", "I need a clickable prototype", "mock this up", "show me what this would look like", "build a UI from this PRD", "make this interactive", "I need something to show stakeholders", or "turn this into a prototype". Also trigger when the user shares a PRD and asks for a visual or interactive representation. Do NOT use for writing PRDs (use prd-builder), reviewing PRDs (use prd-checker), or breaking PRDs into epics for engineering (use prd-decomposer).
---

# PRD Prototyper

Turn a PRD into a clickable, styled UI prototype. Extracts the UX layer from a PRD, generates realistic sample data, and produces either a React artifact you can interact with immediately or a structured prompt for a coding agent to build a fuller application.

## Scope and Pipeline Context

This skill is an optional step in the PRD pipeline, sitting between the checker and the final handoff:

```
prd-builder → prd-checker → [prd-prototyper] → prd-decomposer → SpecKit
    ▲                          (optional)                        OR hand to eng
    │                               │
    └── feedback loops back ────────┘
```

The prototype travels with the PRD — engineering gets both the spec and something they can click through. This gives the dev team UX context that a written spec alone can't convey, and it gives stakeholders something to react to before the team commits to building.

Not every PRD needs a prototype. Simple features, backend-heavy work, or projects with an established UX pattern can skip straight to decomposition or engineering. Prototyping earns its keep when the UX is novel, complex, or politically sensitive — when showing beats telling.

**What this skill does NOT do:**
- It does not build production code — prototypes are for validation, not deployment
- It does not replace engineering review — the prototype demonstrates UX intent, not architecture
- It does not generate backend logic — it's all frontend with mocked data

## The Prototyping Process

### 1. Ingest the PRD

Read the full PRD, but focus on extracting the UX-relevant layers:

- **User Stories** — who uses this and what they're trying to do
- **UX Flows** — entry points, core flows, edge cases, navigation
- **Functional Requirements** — what the UI needs to show, accept, and respond to
- **UI Preferences** — design system, component library, mobile considerations
- **Personas** — different user types that see different things

Technical Considerations, Success Metrics, and Narrative are useful context but don't directly drive the prototype. Skim them for constraints (e.g., "must work on mobile") but don't over-index.

### 2. Scope the Prototype

Ask the PM what they need. Don't assume — different situations call for different scopes:

- **Full app shell** — all major screens, navigation between them, complete flow. Best for stakeholder demos or concept validation of the overall product.
- **Critical flow** — one or two key user journeys end-to-end. Best for validating a specific interaction pattern or testing a hypothesis.
- **Single screen** — one complex screen with interactions. Best for layout validation or when the UX has a high-density view that needs to feel right.

If the PM doesn't have a preference, recommend based on the PRD: small PRDs (1-3 screens) → full shell. Large PRDs (10+ screens) → critical flow, because a full prototype would be unwieldy.

### 3. Choose the Output Mode

Two options, depending on what the PM needs:

**Artifact mode** — a React component rendered directly in Claude.ai. Best for:
- Quick validation ("does this flow make sense?")
- Sharing a link with stakeholders for feedback
- Small-to-medium scope (1-5 screens)
- When the PM wants to iterate live in the conversation

**Agent prompt mode** — a structured prompt document for Claude Code or another coding agent to build a fuller app. Best for:
- Complex multi-screen applications
- When the prototype needs to be deployed or shared outside Claude.ai
- When the PM wants a real repo they can keep iterating on
- Anything that outgrows a single artifact's constraints

If the PM isn't sure, guide them: "Is this something you want to click through right now, or do you need a standalone app you can share a URL for?" Quick validation → artifact. Shareable/deployable → agent prompt.

### 4. Generate Sample Data

Prototypes with "Lorem ipsum" and "User 1" feel fake and undermine the demo. Generate realistic sample data that matches the PRD's domain:

- Use plausible names, dates, numbers, and statuses that fit the product context
- If the PRD describes specific entities (initiatives, devices, tickets, etc.), create 5-10 sample instances with realistic attributes
- Include variety — not all items should be in the same state. Show pending, active, completed, errored states so the prototype demonstrates the full range
- If the PRD references real external systems (Jira, Confluence, etc.), use realistic-looking references (project keys, page titles, etc.)

Sample data should be embedded in the artifact or included in the agent prompt — not left for the PM to create.

### 5. Build It

**For Artifact mode:** Read `references/artifact-guide.md` for constraints and best practices. Build a single React component using Tailwind CSS and available libraries. The artifact should be interactive — clickable navigation, state changes, form inputs that respond. It should look styled and polished, not wireframe-y.

**For Agent prompt mode:** Read `references/agent-prompt-template.md` and generate a structured prompt document. The prompt should include everything a coding agent needs to build the prototype without asking questions — UX flows, screen descriptions, component specs, sample data, interaction patterns, and styling direction.

### 6. Present and Iterate

After delivering the prototype or prompt:

- **Artifact mode:** The PM will click through it and give feedback. Iterate in conversation — "make the sidebar collapsible", "add a confirmation modal for delete", "the table needs a search filter". Each iteration updates the artifact.
- **Agent prompt mode:** Present the prompt as a markdown file. Walk the PM through what it covers. They'll take it to Claude Code or their coding agent of choice.

### 7. Close the Loop

Stakeholder feedback on a prototype almost always surfaces PRD changes — a flow that should work differently, a screen that's missing, a feature that's more important than originally scoped. This feedback needs to flow back into the PRD before the team commits to building anything.

After the prototype has been reviewed, ask:

> Did the prototype surface any changes to the PRD? Things like:
> - Flows that should work differently than what's specced
> - Missing screens or features stakeholders expected to see
> - Priority shifts — something that was P1 is now clearly P0
> - Scope changes — something that was in is now out (or vice versa)
>
> If so, run **prd-builder** in update mode to fold the feedback into the PRD before moving forward.

This step matters because a prototype that diverges from the PRD creates two sources of truth. Close the loop first.

Once the PRD and prototype are aligned, suggest the next step:

> PRD and prototype are in sync. Next:
> - **prd-decomposer** to break the PRD into sequenced epics with SpecKit prompts
> - Or hand the PRD and prototype to engineering directly — the prototype gives devs UX context alongside the spec

## Fidelity Guidance

The prototype should look like a real product, not a wireframe. This means:

- **Styled components** — proper spacing, typography hierarchy, color palette. Use the PRD's design system if specified, otherwise use clean defaults (shadcn/ui is a good baseline for artifacts).
- **Realistic density** — if a dashboard has 6 cards, show 6 cards with real-ish data, not one placeholder card. If a table has 15 rows in production, show 8-10 in the prototype.
- **Interaction feedback** — buttons should have hover states, form fields should validate, navigation should transition. Static screenshots don't validate UX.
- **Responsive if the PRD requires it** — if mobile is mentioned, the prototype should adapt. If desktop-only, don't waste effort on responsive layouts.

What fidelity does NOT mean here: pixel-perfect design-system matching, animation polish, or accessibility compliance. Those matter for production, not for prototype validation.

## Edge Cases

**PRD has no UX section:** Extract what you can from User Stories and Functional Requirements. Ask the PM to describe the key screens/flows verbally. You have enough to prototype if you know: what screens exist, what's on each screen, and how the user moves between them.

**PRD describes API-heavy features with minimal UI:** Some features are mostly backend (sync engines, data pipelines, integrations). For these, prototype the monitoring/management UI — the dashboard that shows status, logs, and configuration. There's almost always a UI surface even for backend-heavy features.

**PM wants to prototype something not in the PRD:** That's fine. The skill works with any feature description, not just formal PRDs. Adapt the process — skip the gap analysis, work from whatever the PM provides.

**The prototype outgrows a single artifact:** If the PM keeps adding screens and the artifact is getting unwieldy, suggest switching to agent prompt mode. "This is getting complex enough that it'd be better as a real app — want me to generate a Claude Code prompt instead?"

## Reference Files

- `references/artifact-guide.md` — React artifact constraints, available libraries, and best practices for Claude.ai
- `references/agent-prompt-template.md` — Template for generating structured coding agent prompts
