# Report Template

Use this exact structure when generating the quality report. The two-part format gives the PM a quick overview (dashboard) and then the detail they need to act on (findings).

## Part 1: Summary Dashboard

```markdown
## PRD Quality Report: [Document Name]

### Agent-Ready Gate: [✅ PASS / ⚠️ CONDITIONAL / ❌ FAIL]
[If CONDITIONAL or FAIL, list the specific blockers here as a short bullet list]

### Summary
- 🔴 Critical Issues: [count]
- 🟡 Warnings: [count]
- 🔵 Info: [count]

### Sections Found: [comma-separated list]
### Sections Missing: [comma-separated list, or "None"]

### Overall Assessment
[1-2 sentence direct, honest verdict]
```

**Overall Assessment examples:**
- "This PRD has solid bones but needs work on requirement specificity before it's engineering-ready."
- "Major structural gaps — missing Non-Goals and Success Metrics entirely. Needs another pass."
- "Clean and well-structured. A few ambiguous requirements to tighten up, but close to ready."
- "The requirements are detailed but the document contradicts itself on scope in three places. Fix the conflicts and this is ready."

Keep the assessment direct. The PM wants to know: can I move forward or not, and what's the biggest problem?

## Part 2: Detailed Findings

Group findings by check category. Within each category, order by severity (🔴 first, then 🟡, then 🔵).

```markdown
### [Check Category Name]

**[🔴/🟡/🔵] [Short title of the issue]**
- **Where:** [Section name and location in the document]
- **What:** [Description of the problem, quoting the specific text]
- **Why it matters:** [Impact if not fixed — engineering confusion, scope creep, agent failure, etc.]
- **Suggested fix:** [Concrete, actionable suggestion for resolving it]
```

**Formatting rules:**

- Lead with 🔴 Critical categories/findings — these block handoff
- Then 🟡 Warning categories — these cause confusion or rework
- Then 🔵 Info categories — nice-to-haves
- Within each severity, order by impact
- Skip categories with zero findings — don't include "No issues found" sections
- Quote the specific problematic text so the PM can find it in their document
- Suggested fixes should be concrete enough to act on, not vague ("be more specific" is not helpful — "specify the sync interval in minutes and define the retry behavior on failure" is)

**For the agent-ready gate blockers:** When the gate result is CONDITIONAL or FAIL, the blockers listed in the dashboard should be the most concise version. The detailed findings section has the full context. Don't duplicate — the dashboard blockers should read like a punch list, and the PM can scroll down for details.
