# Check Rules

Complete ruleset for all 10 check categories. Work through every category against the PRD. Be thorough — read the entire document before making cross-referencing judgments. Only flag issues you can point to specifically in the text.

---

## 1. Structural Completeness

Compare the PRD's sections against the reference template. Flag missing sections with appropriate severity.

**Required sections (🔴 Critical if missing):**
- tl;dr / Executive Summary
- Goals (Business Goals, User Goals, Non-Goals)
- User Stories (with identified personas)
- Functional Requirements (with priority levels)
- User Experience / UX Flows
- Success Metrics

**Expected sections (🟡 Warning if missing):**
- Technical Considerations
- Narrative / User Journey

**Subsection checks:**
- Goals should have Business Goals, User Goals, AND Non-Goals (all three)
- Functional Requirements should have priority levels (P0/P1/P2 or equivalent)
- User Experience should cover entry point, core flow, AND edge cases
- Success Metrics should have measurement methods, not just metric names
- Technical Considerations should cover integration points and constraints

---

## 2. Conflicting Information

Contradictions across sections are the #1 source of engineering confusion and rework. Flag as 🔴 Critical.

**Patterns to catch:**
- Goals state one objective, but requirements describe features serving a different objective
- Non-Goals list something as out of scope, but it appears in Functional Requirements or User Stories
- Success Metrics measure something not addressed in requirements
- Timeline or scope statements in one section conflict with another
- Personas described differently across sections
- Priority levels that conflict (feature called "nice-to-have" in one place and "P0" in another)

For each conflict, quote both contradicting statements and their locations in the document.

---

## 3. Ambiguous Requirements

Scan for language that would leave an engineer or coding agent guessing.

**Vague quantifiers (🟡 Warning):**
- "fast", "quickly", "responsive" — fast means what? Under 200ms? Under 2 seconds?
- "many", "several", "a few", "some" — quantify it
- "most users", "typically", "usually" — what percentage? What's the fallback?
- "easy to use", "intuitive", "simple" — by whose standard?
- "scalable", "performant", "reliable" — define the target

**Undefined terms (🟡 Warning):**
- Jargon or acronyms used without definition
- Feature names referenced but never described
- User types mentioned but not defined as personas

**Missing specifics (🟡 Warning or 🔴 Critical depending on severity):**
- "The system should handle errors appropriately" — what errors? What's appropriate?
- "Users can configure settings" — which settings? What options?
- "Data will be synced" — how often? What triggers sync? What on failure?
- "Notifications will be sent" — via what channel? When? To whom?

**Weasel words (🔵 Info):**
- "should" vs "must" vs "will" — inconsistent obligation language
- "could", "might", "may" — is this a requirement or a suggestion?
- "etc.", "and so on", "and more" — enumerate or cut

---

## 4. Technical Agnosticism

PRDs should describe WHAT and WHY, not HOW. The PM defines the problem and desired outcome; engineering decides implementation. Flag language that prescribes implementation unless it's a genuine constraint.

**Implementation prescriptions (🟡 Warning):**
- Specific frameworks: "Build this in React", "Use PostgreSQL"
- Architectural patterns: "Use a microservices architecture", "Implement with REST APIs"
- Code-level details: "Create a database table with columns...", "Use a cron job to..."
- Specific libraries: "Use Chart.js for visualization"

**Acceptable technical references (don't flag):**
- Existing system names representing real constraints: "Must integrate with Digi Remote Manager"
- Platform requirements affecting UX: "Must work on iOS and Android"
- Security/compliance requirements: "Must support SSO via SAML 2.0"
- Performance targets: "Page load under 2 seconds on 3G"
- References to existing APIs or data sources the feature depends on

The distinction: constraints are fine, prescriptions are not. "Must integrate with our existing REST API" is a constraint. "Build a new REST API using Express.js" is a prescription.

---

## 5. Traceability

Check that the PRD's sections connect logically. Broken traceability means something was added without connecting it to the rest of the document — a common sign of scope creep or incomplete thinking.

**User Stories → Requirements (🟡 Warning if broken):**
- Every user story should map to at least one functional requirement
- Flag orphan stories (stories with no corresponding requirement)
- Flag orphan requirements (requirements that don't trace to any user story)

**Goals → Success Metrics (🟡 Warning if broken):**
- Each business goal should have at least one metric measuring progress toward it
- Each user goal should have a metric or user story validating it
- Flag metrics that don't connect to any stated goal

**Non-Goals → Requirements (🔴 Critical if broken):**
- Nothing in Functional Requirements should contradict a stated Non-Goal
- If something creeps toward a Non-Goal, flag it

**Requirements → UX (🟡 Warning if broken):**
- Key requirements should be reflected in UX flows
- Flag requirements with no UX coverage — the "how does the user actually do this?" gap

---

## 6. Section Completeness

Depth checks within individual sections.

**User Stories:**
- Follow "As a [type], I want to [action], so that [benefit]" format (or equivalent)
- Each story has persona, action, AND benefit — not just persona and action
- Stories aren't too broad ("As a user, I want to manage everything") or too narrow ("As a user, I want to click the blue button")

**Functional Requirements:**
- Each requirement is specific enough to implement and test
- Priority levels assigned
- Error states and edge cases addressed (at least acknowledged)
- No circular or self-referencing requirements

**Success Metrics:**
- Metrics are measurable (not "improve user satisfaction")
- Baseline or target values included
- Measurement method specified (what tool, what event, what query)
- Timeframe for measurement defined

**UX Flows:**
- Happy path fully described
- Error states have defined behavior
- Entry points identified
- Navigation between states is clear

---

## 7. Scope Creep Detection

Look for requirements disproportionate to the stated goals — the "is the juice worth the squeeze" check.

**Scope-to-goal mismatch (🟡 Warning):**
- Requirements introducing significant complexity but only loosely connecting to a stated goal
- Feature groups that could be their own product/initiative
- Requirements serving a goal not listed in the Goals section (hidden goals = hidden scope)

**Creeping sophistication (🟡 Warning):**
- "Phase 1" language implying a much larger roadmap baked into the initial spec
- Requirements stacking multiple behaviors into one item ("and also", "additionally")
- Admin/configuration features exceeding what the core use case needs

**The test:** For each feature group, ask: "If we cut this entirely, does the PRD still achieve its primary business goal?" If yes, it might be scope creep. Flag it and note which goal it serves (if any).

---

## 8. Persona Consistency

Verify that personas flow through the entire document, not just User Stories.

**Persona coverage gaps (🟡 Warning):**
- Personas defined in User Stories but absent from UX Flows (designed for one persona, forgot others)
- Requirements serving a persona not defined in User Stories (phantom persona)
- UX flows assuming a single user type when multiple personas were defined
- Success Metrics only measuring outcomes for one persona

**Persona definition issues (🔵 Info):**
- Personas too similar to each other (could be merged)
- Personas defined with job titles only, lacking context about goals, expertise, or usage patterns
- Personas referenced inconsistently ("admin" in one place, "system administrator" in another)

---

## 9. Testability

For each functional requirement, ask: "Could QA write a test case from this as written?" If not, it's too vague to build against.

**Untestable requirements (🟡 Warning):**
- Requirements with no defined success/failure criteria
- Requirements describing a state ("the system is reliable") rather than a behavior ("the system retries failed requests up to 3 times")
- Requirements where expected output is undefined ("the system processes the data")
- Requirements with no boundary conditions ("supports large files" — how large?)

**Missing acceptance criteria (🟡 Warning):**
- P0 requirements without explicit pass/fail conditions
- User-facing features without defined expected behavior for valid and invalid inputs
- Integration points without defined contract (what goes in, what comes out, what on failure)

The bar: could someone who has never seen this product write an automated test from the requirement alone? If they'd need to ask clarifying questions, the requirement needs work.

---

## 10. Cross-PRD Dependencies

Flag when the PRD references external products, features, or systems that represent delivery dependencies.

**Unvalidated dependencies (🟡 Warning):**
- References to other products/features being "available" or "live" without confirmed timeline
- Assumptions about APIs or data sources owned by another team
- "Requires [X] to be complete first" without linking to the relevant PRD or tracker
- Integration points with systems that may have their own roadmap constraints

**Missing dependency details (🔵 Info):**
- External dependencies mentioned without a named owner or team
- Dependencies without a fallback plan ("if X isn't ready, we will...")
- Shared infrastructure assumptions without confirmation they can handle additional load
