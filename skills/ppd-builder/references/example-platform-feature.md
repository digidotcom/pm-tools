# Example PPD: FleetCore Agent Gateway (Fictional)

A **fictional** example PPD — Meridian Networks, FleetCore, and all competitors named here are invented; any resemblance to real companies is coincidental. Use this to gauge appropriate **tone, depth, and specificity** when drafting PPDs for **platform extensions or developer-facing capabilities** — launches where the buyer builds on the product rather than just operating it.

**What to study here:**
- Q1 handles a **primary + secondary customer** cleanly — the primary is integration builders (partners, MSPs, dev teams), the secondary is day-to-day operators using AI assistants. Each gets its own insights and behavior sub-sections. This is how to do dual-audience positioning without losing sharpness.
- Q2's problem statement follows the Working Backwards template exactly, with specific, data-backed challenges underneath.
- Q3 includes a **comparison table of alternatives** with the winning approach marked — battlecard-ready thinking even when direct competitors are thin.
- Q3 states the first-mover situation explicitly ("no direct competitor yet; the real alternatives are X and Y") instead of leaving named competitors blank.
- Q4 leads with ONE primary benefit and gives the secondary persona its own secondary benefit — short, memorable, campaign-ready.
- Q5 ties a metric to a revenue thesis (subscription stickiness), not just adoption counts.
- The Marketing Handoff shows keyword grouping by intent and per-persona angles for a technical buying committee.

---

# Product Positioning Document: FleetCore Agent Gateway

**Document Owner:** [PM Name]
**Date:** 2026-04-10
**Revision:** 1.0
**Status:** In Review
**Related PRD:** [internal link]
**Primary Audience:** Marketing (Product Marketing, Campaign Marketing, Sales Enablement)

---

## Q1. (Listen) Who is the customer and what insights do we have about them?

### 1. Primary Customer
**Partners, MSPs, and customer dev teams building AI agents** that need to interact with FleetCore Cloud — technical teams automating monitoring, diagnostics, and operational workflows on top of the platform.

**Secondary Customer:** field engineers, fleet administrators, and support teams who want to use AI assistants they already have (chat clients, copilots) to query and manage their FleetCore accounts in natural language.

### 2. Insights on Them

**Primary (agent builders):**
- They're building AI agents for operational automation and see platform connectivity as the gating cost.
- REST APIs work but demand custom integration code, error handling, and ongoing maintenance per project.
- MSPs manage fleets for many end customers — they need one integration that scales across accounts.
- They're actively choosing which platforms to standardize on; native AI connectivity is becoming a selection criterion.

**Secondary (operators):**
- They already use AI assistants daily for docs, troubleshooting, and drafting — extending that to fleet operations is the obvious next step.
- They repeat the same queries constantly ("which units are offline at site X?", "firmware spread across the region?") and would rather ask than click through dashboards.
- They will not write code; adoption must be connect-and-go.

### 3. Their Behavior

**Primary:** evaluate platforms by integration effort and maintenance burden; prefer hosted, key-authenticated services over infrastructure they must run; standardize on open protocols when available; prototype in days and decide fast.

**Secondary:** start the day in dashboards and alerts; adopt tools that remove clicks from repetitive checks; trust grows through read-only use before they attempt actions.

---

## Q2. (Define) What is the prevailing customer problem/opportunity? What data informed this?

### Customer Challenges
Today, teams building AI agents have to write custom REST integrations when they want their agents to work with FleetCore Cloud. Specifically:

- **Integration cost:** wrapping REST for AI consumption means custom code, schema mapping, and error handling — repeated by every builder, for every project.
- **No standard interface:** each AI framework wants tools shaped its own way; builders re-solve the same problem per stack.
- **Adoption barrier:** the integration effort delays or outright kills AI projects that would otherwise deepen platform usage.

### Problem Statement
Today, partners, MSPs, and customer dev teams have to build and maintain custom API integrations when they want AI agents to work with their fleet platform. Customers need a way to connect agents instantly through an open standard, without integration code or infrastructure.

### Why Now — Market Timing
- **Technology inflection:** an open agent-connectivity protocol has emerged as the de facto standard for wiring AI agents to external tools — the ecosystem (clients, frameworks, tooling) reached practical maturity in the last year.
- **Customer behavior change:** operators now arrive already using AI assistants daily; the demand to point those assistants at operational systems is inbound, not pushed.
- **Competitive shift:** no vendor in the segment offers native agent connectivity yet — a first-mover window that will not stay open.

### Supporting Data — Internal Credibility
- Partner advisory sessions: majority of top partners report active AI-agent initiatives that touch fleet data (notes, last two quarters).
- API-key issuance for third-party integrations has accelerated for four consecutive quarters (platform analytics).
- Support logs show operators pasting API responses into AI chat tools manually — the workaround demonstrates the demand.

### Supporting Data — Publishable Proof
- Public developer-preview waitlist count (publishable as a momentum stat at launch).
- [TBD — need: a named partner quote from the preview program with permission to publish.]
- Protocol ecosystem adoption stats from the standard's public registry (third-party, citable).

### Customer Opportunity
If agent connectivity is native, builders ship automation in days instead of integration sprints, MSPs scale one integration across every managed account, and operators get answers by asking — making the platform the natural center of AI-driven operations.

---

## Q3. (Invent) What is the solution? Why is it the right solution to address the customer need versus other alternatives?

### Solution (1-2 sentences)
FleetCore Agent Gateway is a hosted, key-authenticated endpoint that exposes FleetCore Cloud to any standards-compliant AI agent or assistant — no integration code, no infrastructure, connected in minutes.

### Why This Solution vs. Alternative Approaches

| Approach | Verdict | Rationale |
|---|---|---|
| **Hosted standards-based gateway (chosen)** | ✅ | Zero customer infrastructure, works with every compliant client, one integration maintained by us |
| Open-source self-hosted connector | ✗ | Pushes hosting and upgrade burden onto customers; MSPs won't run per-tenant infra |
| Publish SDK + examples only | ✗ | Still integration work per builder; doesn't serve the no-code operator persona at all |
| Embed an assistant inside our own UI | ✗ | Serves operators but not builders; locks value inside our surface instead of meeting agents where they run |

### Named Competitive Products
No direct competitor offers native agent connectivity in this segment yet — that absence is the positioning. The real alternatives customers weigh are **build-it-yourself REST integration** and **waiting**. Adjacent vendors to monitor:

| Competitor | Their Positioning | How We Counter-Position |
|---|---|---|
| Ridgeline Telemetry | "Open API platform" (REST + docs) | An API is homework; a gateway is a plug — minutes to connected vs. weeks of integration |
| Averlane IoT | AI features embedded in their own dashboard | Their AI lives in their UI; ours meets your agents where they already run |

### Key Differentiators
First native, standards-based agent connectivity in the segment; hosted and maintained by us (zero customer infra); same auth and permission model as the platform — agents get exactly the access their key grants, nothing more.

---

## Q4. (Refine) How would we describe the end-to-end customer experience? What is the most important customer benefit?

### Most Important Customer Benefit
Instant AI connectivity — zero integration code, zero infrastructure, zero maintenance.

*(Secondary, for the operator persona: manage your fleet in natural language, from the assistant you already use.)*

### How This Solves the Customer's Pain
Q2's problem is integration cost as the barrier between AI ambitions and fleet data. The gateway deletes the integration step: builders point their agent at an endpoint and authenticate; operators paste a URL and a key into the assistant they already have. Time-to-first-value drops from weeks to minutes, and maintenance drops to zero because we run the surface.

### End-to-End Customer Experience
1. **Discover / Evaluate:** developer docs + a live sandbox; operators discover via in-product callout and community demos.
2. **Acquire / Procure:** included with the platform subscription tier — no separate SKU friction at entry.
3. **Deploy / Install:** generate a scoped key, add the endpoint to any compliant client; first successful query in under ten minutes.
4. **Operate / Manage:** usage visible in the platform's audit log; keys scoped and revocable like any platform credential.
5. **Grow / Expand:** builders graduate from read-only queries to operational actions; MSPs roll the pattern across managed accounts.

### Supporting Customer Benefits
- **Framework-agnostic:** one gateway serves every compliant agent stack — no per-framework work.
- **Security posture preserved:** existing auth, scoping, and audit apply unchanged.
- **MSP leverage:** one integration pattern across all managed end-customers.
- **Future-proof:** as the agent ecosystem grows, connectivity is already in place.

---

## Q5. (Test & Iterate) How will we define and measure success?

### Customer Adoption Metrics
- Active gateway keys per month, split by builder vs. operator use (platform analytics)
- Time-to-first-successful-query for new keys (instrumented funnel)
- Share of top-tier partners with at least one production agent connected

### Revenue Growth
- **Stickiness thesis:** subscription renewal rate for accounts with active gateway usage vs. without — connected agents make the platform harder to leave
- Tier upgrades attributed to gateway usage limits (billing events)

### Strategic Positioning
- First-mover recognition: analyst and press mentions of agent connectivity in the segment
- Developer-community signals: sandbox signups, docs traffic, community integrations published

### Product Portfolio Performance
- Gateway API error rates and latency SLOs (ops dashboards)
- Tool-coverage growth: share of platform capabilities exposed through the gateway per quarter

---

## Marketing Handoff

### Elevator Pitch (30-second verbal)
Everyone's building AI agents — and every one of them stalls at the integration step. FleetCore Agent Gateway connects any standards-compliant agent or AI assistant to your fleet in minutes: no code, no infrastructure, nothing to maintain. Builders ship automation the same week; operators just ask their assistant which units are down.

### Tagline Options (3-5 candidates)
1. Your fleet, one question away.
2. Plug in your agents. Skip the integration.
3. AI-ready in minutes, not sprints.
4. The fastest path from agent to answer.
5. Connect intelligence to infrastructure.

### One-Sentence Headline
Connect AI agents to your fleet in minutes — no integration code required.

### Target Search Keywords & Phrases
**Primary terms (high intent):**
- connect AI agent to fleet management
- [protocol name] fleet platform
- AI assistant device management

**Secondary terms (category / awareness):**
- AI agent integration platform
- natural language fleet operations
- agent-ready IoT platform

**Long-tail / problem-aware terms:**
- how to connect AI assistant to device fleet
- automate fleet monitoring with AI agents
- ask AI which devices are offline

### Publishable Proof Points
- Developer-preview waitlist momentum stat (at launch)
- Protocol ecosystem adoption figures from the public registry (third-party citation)
- [TBD — partner quote pending permission]

### Named Competitors (Battlecard Seed)
| Competitor | Our One-Line Counter |
|---|---|
| Ridgeline Telemetry | An API is homework — we're a plug, connected in minutes |
| Averlane IoT | Their AI is trapped in their dashboard; ours works in the assistant you already use |

### Persona Messaging Angles
| Persona | Primary Concern | Messaging Angle |
|---|---|---|
| Integration builder / partner dev | Integration cost, maintenance burden | Zero-code connectivity; we maintain the surface so your agents just work |
| Fleet operator / field engineer | Faster answers, no new tools to learn | Ask your existing assistant; no dashboards, no code, scoped to your key |
| MSP / executive sponsor | Scale and stickiness across accounts | One integration pattern across every managed account — leverage, not headcount |
