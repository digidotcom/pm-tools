# PPD Template

Use this structure when building the PPD. Create the document during Step 7 (Build), then update it incrementally with targeted edits as gaps are filled.

This template mirrors Digi's official Product Positioning Document (PPD) structure using Amazon's Working Backwards framework, with **marketing-focused enhancements** (Why Now narrative, named competitors, publishable proof split, and a Marketing Handoff appendix).

## Template

```markdown
# Product Positioning Document: [Product or Feature Name]

**Document Owner:** [PM Name]
**Date:** [YYYY-MM-DD]
**Revision:** [1.0]
**Status:** [Draft / In Review / Approved]
**Related PRD:** [Confluence link or N/A]
**Primary Audience:** Marketing (Product Marketing, Campaign Marketing, Sales Enablement)

---

## Q1. (Listen) Who is the customer and what insights do we have about them?

### 1. Primary Customer
[Who they are, where they operate, defining characteristics. Focus on ONE primary segment — sharpness beats breadth. Include industry, company size/type, geographic context, and role/persona where relevant.]

### 2. Insights on Them
[What drives them. Their priorities, constraints, and decision criteria. Who the decision-makers are (technical managers, procurement, architects, etc.). What they value and what they avoid.]

### 3. Their Behavior
[How they buy, deploy, and operate. Planning/deployment cycles. Vendor preferences. Technology preferences (standards-based vs. proprietary, open vs. closed). Tools and platforms they rely on. What "field-proven" looks like to them.]

---

## Q2. (Define) What is the prevailing customer problem/opportunity? What data informed this?

### Customer Challenges
Customers face challenges with:
- **[Challenge 1]:** [Description]
- **[Challenge 2]:** [Description]
- **[Challenge 3]:** [Description]
- **[Challenge 4]:** [Description]

### Problem Statement
Today, [customers] have to [problem/opportunity] when [situation]. Customers need a way to [insert need].

### Why Now — Market Timing
[The external trigger that makes this the right moment. What has changed in the market, technology, regulatory environment, or customer behavior? This is the campaign hook — without it, launches feel generic.]

- **Market/regulatory driver:** [e.g., new compliance deadline, industry shift]
- **Technology inflection:** [e.g., new standard reaching maturity, protocol adoption]
- **Competitive shift:** [e.g., competitor exited category, new entrant disrupted pricing]
- **Customer behavior change:** [e.g., AI adoption curve, procurement pattern shift]

### Supporting Data — Internal Credibility
Data that builds executive and stakeholder confidence. Not necessarily publishable externally.

- [Win/loss data point]
- [Support ticket volume or trend]
- [Sales team feedback / customer call themes]
- [Internal market sizing]
- [Product analytics or usage trend]

### Supporting Data — Publishable Proof
Data marketing can cite externally — in landing pages, press releases, analyst briefings, and paid media.

- [Customer quote with permission to publish]
- [Third-party analyst data with source citation]
- [Public benchmark or industry report]
- [Award, certification, or recognition]
- [Published case study reference]

*If this section is thin, flag it explicitly — thin publishable proof is a gap marketing will feel on launch day.*

### Customer Opportunity
[What becomes possible if this problem is solved. Frame the upside — the "why now" payoff — without naming the solution yet.]

---

## Q3. (Invent) What is the solution? Why is it the right solution to address the customer need versus other alternatives?

### Solution (1-2 sentences)
[High-level description. Not a feature list. This is the elevator pitch the solution needs to survive.]

### Why This Solution vs. Alternative Approaches
- **Considered:** [Alternative approach 1 — e.g., build vs. buy, partner, proprietary] — [rationale for rejection or inferiority]
- **Considered:** [Alternative approach 2] — [rationale]
- **Considered:** [Alternative approach 3] — [rationale]

### Named Competitive Products
The specific competitor products customers evaluate alongside ours. This is battlecard seed content — named products, not generic categories.

Rows come from the PM's named-competitor list — the companies that actually show up in evaluations for *this* product (reuse the list from `product-research` or the PRD when it exists). Prune aggressively — not every PPD competes with every vendor.

| Competitor | Their Positioning | How We Counter-Position |
|---|---|---|
| [Competitor 1] | [How they talk about themselves / their claimed strength for this segment] | [Our counter-message — why we win here] |
| [Competitor 2] | [Their positioning] | [Our counter] |
| [Competitor 3] | [Their positioning] | [Our counter] |

### Key Differentiators
[Why Digi is uniquely positioned to deliver this solution. Ecosystem, IP, installed base, certifications, relationships, distribution, etc. Be specific — "we have a great ecosystem" is not a differentiator; "SOC 2 Type II–certified device management platform with AT&T, T-Mobile, and Verizon carrier certifications on the same device" is.]

---

## Q4. (Refine) How would we describe the end-to-end customer experience? What is the most important customer benefit?

### Most Important Customer Benefit
[One sentence — the single benefit that matters most. Pick one. **This sentence will likely become the campaign headline.** Make it count.]

### How This Solves the Customer's Pain
[2-4 sentences connecting the benefit back to Q2's problem. Why "just works" matters. Time-to-value, operational simplicity, risk reduction, capability unlock, etc.]

### End-to-End Customer Experience
Walk through the journey from first touch to long-term operation:

1. **Discover / Evaluate:** [How customers find and assess the solution]
2. **Acquire / Procure:** [Purchase motion — direct, channel, OEM, kit]
3. **Deploy / Install:** [First-time setup, dev kit experience, documentation, guided setup]
4. **Operate / Manage:** [Day-to-day operation, monitoring, management, scale-out]
5. **Grow / Expand:** [Long-term evolution, additional use cases, roadmap alignment]

### Supporting Customer Benefits
- **[Benefit 1]:** [Description]
- **[Benefit 2]:** [Description]
- **[Benefit 3]:** [Description]
- **[Benefit 4]:** [Description]

---

## Q5. (Test & Iterate) How will we define and measure success?

### Customer Adoption Metrics
- [Metric 1 — e.g., kits sold, new accounts, device activations, module sales]
- [Metric 2]
- [Metric 3]

### Revenue Growth
- [Metric 1 — e.g., product revenue, attach revenue, channel reorders, ARR]
- [Metric 2]
- [Metric 3]

### Strategic Positioning
- [Metric 1 — e.g., standards body participation, media coverage, SEO, analyst coverage, thought leadership]
- [Metric 2]
- [Metric 3]

### Product Portfolio Performance
- [Metric 1 — e.g., roadmap evolution, new SKUs added, support ticket trends, firmware release stability]
- [Metric 2]
- [Metric 3]

---

## Marketing Handoff

**Purpose:** Campaign-ready artifacts synthesized from the 5 Qs above. This is the section Product Marketing, campaign managers, and sales enablement will open first. Everything here should be directly usable — copy-pasteable into a landing page draft, an email nurture, a battlecard, or a pitch deck.

### Elevator Pitch (30-second verbal)
[2-3 sentences, spoken-word cadence. Problem + solution + why-us. Should survive being said out loud without sounding like a doc.]

### Tagline Options (3-5 candidates)
Short, memorable, campaign-worthy. The PM picks; marketing refines; one makes it to market.

1. [Tagline option 1]
2. [Tagline option 2]
3. [Tagline option 3]
4. [Tagline option 4]
5. [Tagline option 5]

### One-Sentence Headline
[Landing page hero / press release lede. Max ~15 words. Leads with the most important customer benefit from Q4.]

### Target Search Keywords & Phrases
SEO and SEM seed list. Group by intent.

**Primary terms (high intent):**
- [Keyword 1]
- [Keyword 2]
- [Keyword 3]

**Secondary terms (category / awareness):**
- [Keyword 1]
- [Keyword 2]
- [Keyword 3]

**Long-tail / problem-aware terms:**
- [Keyword 1]
- [Keyword 2]
- [Keyword 3]

### Publishable Proof Points
Pulled from Q2's "Publishable Proof" — the data, quotes, and recognition marketing can cite externally.

- [Proof point 1 with source]
- [Proof point 2 with source]
- [Proof point 3 with source]

### Named Competitors (Battlecard Seed)
Pulled from Q3. Quick-reference for sales enablement. Keep only the competitors that actually made it through Q3 — don't list vendors that aren't relevant to this product.

| Competitor | Our One-Line Counter |
|---|---|
| [Competitor 1] | [One-line counter-position] |
| [Competitor 2] | [One-line counter-position] |
| [Competitor 3] | [One-line counter-position] |

### Persona Messaging Angles
How we talk about this to different roles in the buying committee. Each angle is a different emphasis of the same underlying positioning.

| Persona | Primary Concern | Messaging Angle |
|---|---|---|
| [Technical Architect / Network Engineer] | [What they worry about] | [How we talk to them — emphasize performance, interoperability, control] |
| [Procurement / Buyer] | [What they worry about] | [How we talk to them — emphasize TCO, vendor stability, terms] |
| [Business Owner / Executive Sponsor] | [What they worry about] | [How we talk to them — emphasize outcomes, risk reduction, strategic fit] |
```

## Section Notes

- **Q1 Primary Customer:** A single segment, not a list. Sharpness beats breadth.
- **Q2 Problem Statement:** The "Today, [X] has to [Y] when [Z]" template is non-negotiable. It's the signature of Working Backwards — and it often becomes the landing page opener.
- **Q2 Why Now:** External trigger, not internal schedule. Regulatory, technology, competitive, or behavioral shift. This is the campaign hook.
- **Q2 Data Split:** Internal credibility vs. publishable proof. Marketing can only cite publishable proof externally. If publishable proof is thin, flag it — it's a launch-day gap.
- **Q3 Named Competitors:** Real products/vendors from the BU's competitive landscape (collected from the PM or reused from prior research) — not generic categories. Battlecard seed.
- **Q3 Alternatives:** Approach-level rationale (build vs. buy, proprietary vs. standards). Different axis from named competitors.
- **Q4 Most Important Benefit:** Pick ONE. This becomes the campaign headline.
- **Q5 Four Categories:** Use Digi's four standard categories. Don't invent new ones.
- **Marketing Handoff:** Synthesized LAST from everything above. Not new data — re-presentation in campaign-usable form. Every artifact should be something marketing can use tomorrow.

## Gap Markers

When drafting from incomplete input, mark gaps inline:

```markdown
### Publishable Proof
- [TBD — need: customer quotes with permission to publish, or third-party analyst data. Current proof is all internal. Marketing will feel this gap on launch day.]
```

This gives the PM something specific to react to rather than a blank section.

## Revision Conventions

When updating an existing PPD, bump the revision number:

- **Minor corrections, typos, language tightening:** 1.0 → 1.1
- **Substantive positioning or scope changes:** 1.0 → 2.0
- **Net-new sections or major narrative shifts:** 1.0 → 2.0

Note the change in a Revision History table at the top of the document:

```markdown
| Rev | Date | Author | Description |
|-----|------|--------|-------------|
| 1.0 | 2026-01-15 | [PM Name] | Initial draft |
| 1.1 | 2026-02-03 | [PM Name] | Tightened Q1 primary customer per PMM feedback |
| 2.0 | 2026-03-10 | [PM Name] | Repositioned Q3 — added Cradlepoint as named competitor |
```
