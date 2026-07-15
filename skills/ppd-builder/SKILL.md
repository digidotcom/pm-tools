---
name: ppd-builder
description: Builds Digi Product Positioning Documents (PPDs) using Amazon's Working Backwards framework. The PPD's primary audience is marketing — it feeds messaging, campaigns, landing pages, battlecards, and sales enablement. Ingests existing PRDs, competitive analysis, customer interviews, and other context — then interviews only for gaps. Produces a filled PPD (5 Working Backwards questions plus a Marketing Handoff appendix with elevator pitch, taglines, headline, keywords, proof points, and named competitors). Also handles updates. Use when the user wants to create a PPD, write product positioning, draft a Working Backwards document, build marketing positioning for a launch, or update an existing PPD. Trigger on phrases like "write a PPD", "build a PPD from this PRD", "I need positioning for", "draft a Working Backwards doc", "GTM positioning for", or when the user shares a PRD and wants positioning formalized. Do NOT use for PRDs (use prd-builder) or PRD reviews (use prd-checker).
---

# PPD Builder

Build Digi Product Positioning Documents (PPDs) using Amazon's Working Backwards framework.

## Who This Doc Is For — Read This First

**The PPD's primary audience is Marketing.** It's the source document Product Marketing uses to build:

- Messaging frameworks and value propositions
- Landing page copy, press releases, launch announcements
- Battlecards and competitive content
- Campaign narratives (email nurture, ABM, paid media, webinars)
- Sales enablement materials and pitch decks
- SEO/SEM keyword strategy
- Analyst briefings and media talking points

**This framing changes how everything is written.** The PPD is not a spec (that's the PRD). It's not an engineering document. It's a positioning and narrative document that needs to survive being read by marketers, salespeople, and executives — and needs to hand off campaign-ready artifacts.

Three rules that follow from this:

1. **Market-facing language, not spec language.** "REST API with OAuth2" is PRD language. "Secure programmatic access for integration partners" is PPD language. Translate feature detail into customer value.
2. **Name the competition.** Marketing can't build battlecards without named competitors. Generic "alternatives" don't cut it — name the specific companies and products from the BU's competitive landscape. The list comes from the PM: if `product-research` already ran for this initiative (or the PRD carries competitive material), reuse that named-competitor list instead of re-asking; otherwise ask which companies show up in real customer evaluations.
3. **End with a Marketing Handoff.** The final section synthesizes campaign-ready artifacts (elevator pitch, taglines, headline, keywords, proof points) from the 5 Qs. This is the section marketing opens first.

## Scope and Pipeline Context

The PPD is a **downstream-of-PRD** artifact in Digi's GTM flow. The PRD says *what* to build; the PPD says *who it's for, why it wins, and how we talk about it*. Most PPDs at Digi are built while — or after — the PRD is being written, and feed directly into marketing's campaign and enablement work.

```
  PRD (prd-builder)
        │
        ├──► PPD (this skill) ──► Marketing Handoff ──► Campaigns
        │                                             ──► Battlecards
        │                                             ──► Sales enablement
        │                                             ──► Landing pages
        │
        └──► prd-checker ──► prd-decomposer ──► Engineering
```

**When a PRD exists, ingest it first.** The PRD carries most of what the PPD needs:

| PRD Section | Maps to PPD |
|---|---|
| User Stories + personas | Q1 (Primary Customer, Insights, Behavior) |
| Business Goals + user pain points | Q2 (Problem/Opportunity) |
| Functional Requirements (high level) | Q3 (Solution) |
| Narrative + UX flows | Q4 (End-to-end experience + key benefit) |
| Success Metrics | Q5 (measurement) |

A completed PRD usually gets a PPD 60-80% drafted on the first pass. Spend the interview time on **positioning-specific gaps that marketing cares about**: named competitors, why-now timing, publishable proof, and campaign-ready language.

When the PPD is complete, the Marketing Handoff appendix is what Product Marketing actually reaches for.

## Two Modes: New Build vs. Update

### New Build
The PM wants to create a PPD from scratch, from a PRD, or from any combination of source material. Follow the full build process below.

### Update
The PM has an existing PPD and needs to modify it. Updates typically come from:

- **Market feedback** — sales, channel, or field input changed the positioning
- **Competitive shift** — a competitor moved and the differentiation needs to sharpen
- **Scope change** — the underlying product scope changed, cascading into positioning
- **Leadership or marketing feedback** — VP/CMO/PMM pushed back on narrative, metrics, or campaign artifacts

Process:

1. **Ingest the existing PPD and the change request** — understand both the current state and what needs to change.
2. **Assess impact** — does the change affect just one question, or cascade? A shift in primary customer (Q1) almost always changes Q2 data, Q5 metrics, and the Marketing Handoff.
3. **Present the impact** — "This change touches Q1, Q2, and the Marketing Handoff taglines. Q3's solution rationale still holds. Here's what I'd update." Let the PM confirm before editing.
4. **Make surgical edits** — edit only the affected sections in place. Don't rewrite what doesn't change.
5. **Re-check consistency** — after edits, verify cross-question alignment and that the Marketing Handoff still reflects the updated positioning.

## The Build Process (New Build)

### 1. Ingest Everything

Read the PM's prompt and all provided files before doing anything else. Source material could be a PRD, product brief, competitive analysis, customer interview notes, win/loss data, roadmap slides, meeting transcripts, or tech specs.

Absorb it all. Don't summarize it back.

### 2. Identify the Product Context

Before drafting, confirm three things — either from input or by asking:

- **Product/feature name** — the PPD title (e.g., "Digi EX50 5G", "Digi IX40", "DRM MCP Server", "SOC 2 for DRM & Genesis", "Remote Reach")
- **Launch context** — new product, new feature on an existing product, new variant/SKU, or repositioning?
- **Primary market segment** — if the PRD covers multiple segments, which one is the PPD focused on? PPDs work best when sharpened to a single primary customer segment.

If any are ambiguous, ask — this affects every downstream answer and all Marketing Handoff artifacts.

### 3. Map to Template

Map the provided content to the PPD template sections (see `references/ppd-template.md`). Assess each as:

| Status | Meaning | Action |
|--------|---------|--------|
| **Covered** | Input clearly addresses this section | Draft it — no questions needed |
| **Partial** | Some content exists but incomplete or vague | Draft what's there, ask about the gaps |
| **Empty** | Nothing in the provided material | Ask targeted questions from `references/question-bank.md` |

### 4. Gap Analysis

Present the PM with a structured coverage report using this exact format:

> **Coverage Report — [Product Name] PPD**
>
> | Section | Status | Notes |
> |---|---|---|
> | Q1. Listen (Customer + Insights + Behavior) | ✅ Covered | PRD personas map cleanly |
> | Q2. Define (Problem + Data + Why Now) | 🟡 Partial | Problem clear, need supporting data and why-now narrative |
> | Q3. Invent (Solution + Alternatives + Named Competitors) | 🟡 Partial | Solution described; named competitors missing |
> | Q4. Refine (Experience + Key Benefit) | ✅ Covered | PRD narrative + UX flows carry this |
> | Q5. Test & Iterate (Success Metrics) | ❌ Empty | No GTM metrics in source material |
> | Marketing Handoff (Pitch, Taglines, Headline, Keywords, Proof) | ⏳ Last | Synthesized after everything else |
>
> I'll draft Q1 and Q4 now, then we'll work through the gaps. Marketing Handoff is built last. Sound good?

Wait for confirmation before proceeding.

### 5. Draft First, Ask Second

This is the core principle: **if you have 60% or more of a section's content, draft it and confirm — don't interview.**

- **Covered sections:** Draft the full answer from the PM's input. Present for confirmation. Move on.
- **Partial sections:** Draft what you can, then ask only about what's missing. Frame gaps as specific questions, not open-ended prompts. "You've described the solution, but not who we're positioning against — which specific companies show up in customer evaluations for this?" beats "Tell me about competitors."
- **Empty sections:** Fall back to targeted questions from `references/question-bank.md`. Ask 3-5 at a time, synthesize, confirm, move on.

**The draft-first rule matters because** PMs react better to a draft than to abstract questions. See the two calibration examples in `references/` — `example-compliance.md` (compliance/trust positioning) and `example-platform-feature.md` (developer-facing platform feature) — for tone, depth, and structure.

### 6. Use Working Backwards Language (With Marketing in Mind)

The PPD is explicitly Amazon's Working Backwards framework. Respect its conventions, but remember marketing is the reader:

- **Q2 uses a specific template sentence:** "Today, [customers] have to [problem/opportunity] when [situation]. Customers need a way to [insert need]." Non-negotiable. This sentence often becomes the opening line of a landing page.
- **Q2 does not name the solution.** Keep the solution out until Q3.
- **Q2 separates internal credibility from publishable proof.** Internal data (win/loss, support tickets, sales quotes) builds executive confidence. Publishable proof (customer quotes with permission, third-party analyst data, public benchmarks) is what marketing can cite externally. Keep them separate.
- **Q2 includes a "Why Now" narrative.** Market timing is the campaign hook — regulatory driver, tech inflection, competitive shift, customer behavior change. Without it, launches feel generic.
- **Q3 names competitors.** Not "alternatives considered" — named competitive products from the BU's landscape, collected from the PM (or reused from prior research). Specific products or vendors, not generic categories like "enterprise routers." This is battlecard seed content.
- **Q3 solution is 1-2 sentences at high level.** Not a feature list. Feature detail belongs in the PRD.
- **Q4 focuses on the most important customer benefit.** Pick ONE. This becomes the campaign headline.
- **Q5 uses the four Digi categories:** Customer adoption, Revenue growth, Strategic positioning, Product Portfolio Performance. Don't invent new categories.
- **Marketing Handoff is synthesized last.** It pulls from everything above — elevator pitch, taglines, one-sentence headline, keywords, proof points, named competitors, persona messaging angles.

### 7. Build the PPD

Create the PPD as a **markdown file** (`.md`). Build it section by section:

- Start with sections that have the most complete input — build momentum.
- Create the file after writing Q1, then update it incrementally with targeted edits.
- For skipped sections, include a `[TBD]` placeholder with a note about what's needed.
- Use the structure in `references/ppd-template.md` exactly.
- **Write the Marketing Handoff last** — it's a synthesis of everything else.

### 8. Review and Iterate

Present the full PPD and do a self-check:

- **Narrative consistency:** Q1's customer → Q2's problem → Q3's solution → Q4's experience → Q5's metrics should tell one coherent story.
- **Q2 sentence template:** Applied correctly, no solution leakage.
- **Q2 Why Now:** Present and specific — not a generic "the market is growing."
- **Q2 data split:** Internal credibility and publishable proof are clearly separated.
- **Q3 named competitors:** Real product names, not generic categories.
- **Q4 one benefit:** Headline benefit is singular and sharp.
- **Q5 measurability:** Metrics are measurable with a clear method ("market awareness" isn't measurable; "SEO impressions on product page" is).
- **Marketing Handoff coverage:** All artifacts present (pitch, taglines, headline, keywords, proof, competitors, persona angles). Each one is something marketing can actually use tomorrow.
- **Specificity:** Vague claims flagged. "Scalable" and "easy to use" are near-meaningless without specifics.

Ask the PM to review. Iterate with targeted edits — change only what the feedback touches.

### 9. Handoff

When the PM is satisfied, suggest next steps:

> PPD looks solid. The Marketing Handoff appendix is the section Product Marketing will open first. Typical next steps:
> - Share with Product Marketing to drive the messaging framework, landing page copy, and battlecard
> - Feed the PPD into the **GTM Launch Template** (xlsx) for launch planning
> - Use it as source material for the **Concept Checkpoint** deck
> - If this is a new product, the PPD also anchors the launch press release and analyst briefing

Don't push.

## Best Practices

**Draft-first is the default.** Interview is the fallback. If you're asking more than 10 questions total across the entire PPD, you're probably under-leveraging the PM's input — especially if a PRD exists.

**Primary customer = singular.** If the PM lists three customer segments in Q1, push back: the PPD is sharpest with one primary. Others can be mentioned as secondary. This is the single most common failure mode.

**Positioning language ≠ engineering language.** If the PM's input is PRD-shaped (feature lists, technical specs), translate into market-facing value language every time.

**Named competitors are non-negotiable.** If the PM says "the alternatives are a mix of options from various vendors," push: which specific products are on the shortlist in real customer evaluations? Marketing needs names.

**Publishable proof is often the thinnest section.** PMs have plenty of internal data but few quotable assets. Flag this explicitly — "You've got strong internal credibility data, but we don't have publishable proof. That's a gap marketing will feel immediately."

**Why Now gets skipped a lot.** Market timing is the campaign hook. If a PM says "well, we've been planning this for a while," that's not a why-now — that's an internal schedule. Push for the external trigger.

**Q3 alternatives rationale feeds battlecards.** PMs skip or weak-sauce this. Push: what specifically makes us win against each named competitor in this segment? The answer is battlecard content.

**Marketing Handoff first draft will look rough.** Taglines and headlines are hard. Generate 3-5 options per artifact, let the PM react and kill the weak ones, don't try to nail it on the first shot.

**Contradictions in source material:** Flag explicitly. Don't silently pick a version — ask which is current.

## Reference Files

- `references/ppd-template.md` — The complete PPD markdown template plus the Marketing Handoff appendix
- `references/question-bank.md` — Targeted questions per section, including the marketing-focused gaps
- `references/example-compliance.md` — Filled PPD for a compliance/trust launch (fictional company; full marketing-enhanced template)
- `references/example-platform-feature.md` — Filled PPD for a developer-facing platform feature (fictional company; dual-persona positioning)
