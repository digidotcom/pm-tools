---
name: product-research
description: Orchestrates the pm-tools research agents — market-analyst, competitive-researcher, research-fact-checker — to produce verified market and competitive research for a product initiative. Interviews the PM for BU context FIRST (named company competitors, vertical segments, problem statement), then dispatches the research agents in parallel and fact-checks their output. Use when the user wants market research, competitive research or intel, "research this initiative", "size this market", "what are competitors doing here", "run the research agents", or research to feed a PRD or PPD. Do NOT use for building the PRD itself (use prd-builder), positioning (use ppd-builder), or fact-checking an existing document alone (invoke the research-fact-checker agent directly).
---

# Product Research

Run the research pipeline for a product initiative: gather the BU context conversationally, dispatch `market-analyst` and `competitive-researcher` in parallel, fact-check the results, and hand verified reports to the PM.

**Why this skill exists:** the research agents run autonomously — they cannot interview the PM mid-flight. A wrong competitor list or wrong vertical set means 30 minutes of confidently wrong research. So the interview happens HERE, before anything dispatches.

## 1. Gather the Brief

Collect these before dispatching. Pull what you can from the conversation and any files the PM pointed at — ask only for what's missing, 3-5 questions at a time.

- **Problem statement** — the initiative's problem statement document (typically `<initiative>/docs/problem-statement.md`). If the PM has no document, capture their verbal description into one (a few paragraphs: problem hypothesis + strategic fit) and save it — the agents need a file to read.
- **Named company competitors** — the companies the BU actually sees in deals for this capability. **Companies, not technology stacks** — this rule is law. If the PM offers a generic category ("edge platforms", "the usual router vendors"), push: *which specific companies show up in customer evaluations?* Split into PRIMARY (direct, deep analysis) and SECONDARY (adjacent, brief analysis) if the PM can; a flat list is acceptable. If the PM names pure-software platforms as competitors while the BU ships hardware-anchored products, flag the framing and move those to build-vs-partner candidates.
- **Vertical segments** — the vertical markets the BU sells into (e.g., industrial IoT, transportation/fleet, distributed branch). Market sizing anchors to these, never to a floating tech-category market.
- **BU position and exclusions** — one or two lines on what the BU makes/sells, plus markets explicitly out of scope.
- **Build-vs-partner candidates** *(optional)* — adjacent software platforms worth evaluating as OEM/integrate/partner options. If the PM has none in mind, the competitive-researcher will surface candidates during research.
- **Output folder** — default `<initiative>/research/` next to the problem statement.

## 2. Confirm the Brief

Play the brief back in one compact block (competitors, verticals, position, candidates, paths) and get a yes before dispatching. This is the last cheap moment to fix scope.

## 3. Dispatch Research (parallel)

Launch **market-analyst** and **competitive-researcher** as subagents *in the same message* so they run concurrently. Each invocation prompt must be self-contained — the agent sees nothing of this conversation. Include in each:

- Problem statement path
- The BU's vertical segments
- The competitor list (with PRIMARY/SECONDARY split)
- BU position and exclusions
- Build-vs-partner candidates (competitive-researcher only)
- Output path: `<research-folder>/market-research-raw.md` / `<research-folder>/competitive-research-raw.md`

Expect these to take a while — they each run dozens of web searches.

## 4. Fact-Check

When both raw reports exist, dispatch **research-fact-checker** on the research folder ("fact check all raw files"). It produces a `*-factcheck-report.md` and a `*-verified.md` per raw file.

## 5. Deliver

Report back to the PM:

- The file paths (raw, factcheck report, verified) — the **verified** versions are the ones to use
- Top 5 findings across both reports
- Fact-check summary (claims verified / corrected / removed)
- Next step: feed the verified reports into `prd-builder` or `ppd-builder` as source material

## Partial Runs

The PM may want only one leg ("just competitive research", "just size the market"). Run only that agent — same brief-gathering discipline, then fact-check the single output. Don't run agents the PM didn't ask for.
