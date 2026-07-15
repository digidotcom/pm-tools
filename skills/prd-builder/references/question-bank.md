# Question Bank

Targeted questions for filling PRD gaps. Use during Step 5 (Gap Analysis & Interview) — only ask questions for sections the PM's input didn't cover. Don't run through the full list; pick what's actually missing.

## How to Use This Reference

After drafting the PRD from available input, you'll have sections marked Complete, Partial, or Empty:

- **Complete sections** — skip entirely, no questions needed
- **Partial sections** — scan the questions below and ask only what fills the specific gap
- **Empty sections** — use the full question set for that section, but still batch 3-5 at a time

---

## Goals

### Business Goals

**External projects:**
- What business outcomes does this drive? (revenue, retention, efficiency, market position)
- How does this align with company/team OKRs?
- What's the expected business impact? Can we quantify it?

**Internal projects:**
- What internal problem does this solve?
- Who benefits? (which teams, roles, or workflows)
- What efficiency or capability does this unlock?
- What's the business driver?
  - **Time savings** — hours/week saved, manual steps eliminated
  - **Cost savings** — replacing or reducing 3rd-party tool spend
  - **Feasibility testing** — proving a concept before investing in customer-facing version
  - **Risk reduction** — catching issues earlier, improving quality
  - **Enablement** — giving a team a capability they don't have today
- Can we quantify the impact? (dollars, hours, error rates)

### User Goals
- What does success look like for the user?
- What pain points does this eliminate?
- What new capabilities does this unlock for them?

### Non-Goals
- What are we explicitly NOT doing?
- What adjacent problems will we ignore for now?
- Any common assumptions to call out as out of scope?

**Synthesis:** 3-5 bullet points per goal type.

---

## User Stories

**Identify personas:**
- Who are the distinct user types? (roles, contexts, expertise levels)
- For each persona, what are their primary jobs-to-be-done?
- Are there admin/power user scenarios separate from regular users?

**Per persona:**
- What actions do they need to take?
- What benefit do they get from each action?
- What's their current workaround without this feature?

**Synthesis:** "As a [USER TYPE], I want to [ACTION], so that [BENEFIT]." Group by persona, 3-7 stories each.

---

## Functional Requirements

**Scope the feature set:**
- What are the core capabilities? (MVP must-haves)
- What's nice-to-have vs. must-have?
- What product areas does this touch?

**Per feature area:**
- What specifically does this feature do?
- What inputs does it need? What outputs does it produce?
- What happens in error states?
- Edge cases or special conditions?

**Synthesis:** Group by product area, then priority (P0/P1/P2).

---

## User Experience

**Entry point:**
- How do users discover or access this?
- Is onboarding or first-time experience needed?
- What triggers them to arrive here?

**Core flow:**
- Walk through the happy path step by step
- What decisions at each step?
- What feedback does the user receive?

**Edge cases:**
- Error states that need handling?
- Bad input scenarios?
- Power user flows?

**UI constraints:**
- Design system or component library in use?
- Mobile considerations?
- Accessibility requirements?

**Synthesis:** Numbered steps with UI elements, validation rules, and navigation.

---

## Technical Considerations

**Architecture:**
- What existing systems does this integrate with?
- Frontend framework preferences/constraints?
- New infrastructure needed?

**Data:**
- What data does this feature need and where does it come from?
- New data models required?

**API:**
- New endpoints needed?
- Auth/authz requirements?
- Rate limiting or performance concerns?

**Constraints:**
- Known technical debt to work around?
- Security or compliance requirements?
- Scalability expectations?

**Synthesis:** UI architecture, API/backend needs, performance considerations, integration points.

---

## Success Metrics

- How will we know this succeeded?
- What user behaviors indicate adoption?
- What business metrics should move?
- Baseline we're comparing against?
- How will we measure? (existing instrumentation or new?)
- Timeframe for evaluating success?

**Synthesis:** 4-6 metrics across user-centric, business, and technical categories. Include measurement method.

---

## Narrative

- Describe a real user (or archetype) who would use this
- What's their day like before this feature?
- What triggers them to need it?
- Walk through their experience using it
- What's different afterward?

**Synthesis:** 200-300 word story showing problem → solution → outcome.

---

## tl;dr

No interview needed — synthesize from all completed sections. Problem + key benefits + core features + target audience in 2-3 sentences.
