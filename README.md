# pm-tools

AI tooling for Digi product managers. First up: **prd-workflow** — a PRD pipeline of five skills that take a feature idea from rough notes to an agent-ready spec, plus marketing positioning.

```
prd-builder → prd-checker → [prd-prototyper] → prd-decomposer → SpecKit / engineering
      │
      └──► ppd-builder ──► Marketing Handoff
```

## The skills

- **prd-builder** — builds a complete PRD from whatever you have: notes, transcripts, Slack threads, a brain dump. Drafts first, interviews only for gaps. Also handles updates when scope changes.
- **prd-checker** — lints the PRD: completeness, contradictions, vague requirements, scope creep. Ends with an agent-ready gate check — is this specific enough for a coding agent to build from?
- **prd-prototyper** — turns the PRD into a clickable prototype (React artifact or a build prompt for a coding agent) so stakeholders react to something real before engineering commits.
- **prd-decomposer** — breaks a checked PRD into dependency-ordered epics with GitHub SpecKit `/specify` and `/plan` prompts. For SpecKit-driven builds.
- **ppd-builder** — builds a Product Positioning Document (Amazon Working Backwards) from the PRD, ending in a campaign-ready Marketing Handoff. Named competitors come from you (or get reused from a prior product-research run) — companies, not generic categories.

Start with `prd-builder` — just say "let's write a PRD" and paste in what you have.

## Research: one skill + three agents

Say "research this initiative" (or "size this market", "what are competitors doing") and the **product-research** skill runs the pipeline: it interviews you for your BU's context first — **named company competitors** (companies, not technology stacks), vertical segments, problem statement — then dispatches the research agents in parallel and fact-checks their output.

The agents (each runs in its own context and writes a report file):

- **market-analyst** — market research anchored to *your BU's* vertical segments. Sizes the vertical first, the capability-slice second — never a floating tech-category number.
- **competitive-researcher** — feature-specific competitive intel on your named competitors, plus a Build-vs-Partner analysis of adjacent software platforms as data for engineering.
- **research-fact-checker** — reads a research report, verifies every factual claim via web search, and produces a claim-by-claim audit plus a corrected/qualified version. Point it at any `*-raw.md`.

Flow: product-research (interview) → market-analyst + competitive-researcher (parallel) → research-fact-checker → the verified reports feed `prd-builder`/`ppd-builder` as source material.

## Install

### Claude Code (CLI or desktop app)

```
claude plugin marketplace add digidotcom/pm-tools
claude plugin install prd-workflow@pm-tools
```

Update later with `claude plugin update prd-workflow`.

### Codex CLI, Cursor, Gemini CLI, and other harnesses

The skills in `skills/` are standard [Agent Skills](https://agentskills.io) (`SKILL.md` folders) — the open format read by 30+ tools. Copy the skill folders into your tool's skills directory (e.g. `~/.gemini/skills/`, or your tool's equivalent). Native plugin manifests for other harnesses can be added to this repo on request — open an issue naming your tool.

## Maintainers

Owner: Josh Flinn (joshua.flinn@digi.com). Edit skills in place, bump `version` in `.claude-plugin/plugin.json` (CalVer: `YYYY.MM.DD`), validate with `claude plugin validate .`, commit. Installed copies pick up changes via `claude plugin update prd-workflow`.
