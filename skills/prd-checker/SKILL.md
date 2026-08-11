---
name: prd-checker
description: PRD quality checker and linter that reviews Product Requirements Documents for completeness, consistency, and clarity. Includes an agent-ready gate check — a go/no-go assessment for whether the PRD is specific enough for a coding agent (Claude Code, Gemini, etc.) to execute against. Use this skill whenever the user asks to review, check, lint, audit, critique, or validate a PRD — or when they share a PRD and ask for feedback, issues, problems, gaps, or improvements. Also trigger when the user says things like "does this PRD look good", "what's missing from this spec", "review my requirements doc", "find problems in this PRD", "is this ready for engineering", "is this ready for an agent", "check this spec", "lint my PRD", or "audit my requirements". Works on any PRD format but checks against a proven template structure. Do NOT use for creating or writing PRDs from scratch (use prd-builder for that) or breaking PRDs into epics (use prd-decomposer).
---

# PRD Checker

Review Product Requirements Documents for quality issues and produce an actionable report with severity-rated findings. Think of it like a code linter, but for product specs. It catches problems before a PRD reaches engineering or a coding agent — conflicting information, vague requirements, missing sections, scope creep, untestable requirements, and traceability gaps. Culminates in an agent-ready gate check.

## Scope and Pipeline Context

This skill is the second step in the PRD pipeline:

```
prd-builder → prd-checker → [prd-prototyper] → prd-decomposer → SpecKit
    ▲              │           (optional)                        OR hand to eng
    │              │
    └── fails? ────┘  back to builder to fix issues
```

1. **PRD Builder** (`prd-builder`) → creates and maintains the PRD
2. **PRD Checker** (this skill) → reviews for quality and agent-readiness
3. **PRD Prototyper** (`prd-prototyper`) → *(optional)* builds a clickable prototype for stakeholder validation
4. **PRD Decomposer** (`prd-decomposer`) → breaks into sequenced epics with SpecKit prompts — OR hand the PRD (and prototype) to engineering directly

The checker sits early in the pipeline for a reason. Issues caught here are cheap to fix — the PM takes the report back to `prd-builder` in update mode and fixes a few paragraphs. The same issues caught during decomposition mean re-scoping epics and rewriting SpecKit prompts. Catching them during a prototype demo means stakeholders reacting to the wrong thing. Fix it now, save pain later.

**What this skill does NOT do:**
- It does not rewrite the PRD — it flags problems and suggests fixes so the PM decides what to act on
- It does not evaluate whether the product idea is good — it checks whether the document communicates the idea clearly
- It does not grade or score numerically — severity counts are informational, not a rating
- It does not check grammar or writing style unless ambiguity results from poor writing

## Severity Levels

All findings use three severity levels:

| Severity | Meaning | Impact |
|----------|---------|--------|
| 🔴 Critical | Blocks engineering handoff or causes confusion/rework | Must fix before decomposition |
| 🟡 Warning | Creates ambiguity or gaps that will surface during build | Should fix, may cause rework |
| 🔵 Info | Polish items, style suggestions, minor inconsistencies | Nice to fix, won't block |

## The Review Process

### 1. Ingest the PRD

Accept the PRD in any format — uploaded file (.docx, .md, .pdf, .txt), pasted text, or a link (if accessible via tools). Read the full contents before starting analysis. Don't start checking until you have the complete document.

### 2. Run All Checks

Work through every check category in `references/check-rules.md` against the PRD. Be thorough — read the entire document before making cross-referencing judgments. Only flag issues you can point to specifically in the text.

The check categories cover:

1. **Structural Completeness** — missing sections, missing subsections
2. **Conflicting Information** — contradictions across sections
3. **Ambiguous Requirements** — vague language, undefined terms, missing specifics
4. **Technical Agnosticism** — implementation prescriptions vs. legitimate constraints
5. **Traceability** — stories ↔ requirements ↔ goals ↔ metrics connections
6. **Section Completeness** — depth checks within each section
7. **Scope Creep Detection** — requirements disproportionate to stated goals
8. **Persona Consistency** — personas flowing through the entire document
9. **Testability** — could QA write a test case from each requirement **and each user story** as written?
10. **Cross-PRD Dependencies** — unvalidated external dependencies

Read the full rules before starting. Each category has specific patterns to look for, severity guidance, and examples of what to flag vs. what to skip.

### 3. Agent-Ready Gate Check

After running all check categories, perform a final go/no-go assessment calibrated for coding agents (Claude Code, Gemini, etc.). This is the highest bar for clarity — an agent can't ask follow-up questions, read between the lines, or rely on institutional knowledge.

**A PRD passes the gate when ALL three tests pass:**

**Specificity test:**
- Every functional requirement describes a concrete behavior with defined inputs, outputs, and error states
- No requirement relies on implied context or tribal knowledge ("the usual flow", "like we do in the other product")
- Quantitative targets exist where applicable (performance, limits, thresholds)

**Completeness test:**
- Every user-facing feature has a described UX flow with entry point, happy path, and error handling
- Edge cases are enumerated, not deferred ("handle edge cases appropriately" = instant fail)
- Enough context that someone with zero background could understand what to build
- Every user story carries acceptance criteria, or the PRD states explicitly that pass/fail conditions live in the Functional Requirements and the requirements actually cover every story. Silence on this is a gap, not a convention.
- Stories are individually addressable (story IDs or stable headings) so downstream tickets can trace back to them

**Unambiguity test:**
- Zero 🔴 Critical issues from the checks
- No requirement uses ambiguous language that survived the ambiguity check
- Priority levels are clear and don't conflict

**Gate results:**
- ✅ **PASS — Agent-ready.** Specific enough for a coding agent. Remaining warnings are polish items.
- ⚠️ **CONDITIONAL — Close but not ready.** List the specific blockers that would trip up an agent.
- ❌ **FAIL — Needs significant work.** Multiple critical issues or structural gaps. List top 3-5 blockers.

### 4. Generate the Report

Produce the report using the format in `references/report-template.md`. Two parts: a Summary Dashboard (severity counts, gate result, sections found/missing, overall assessment) and Detailed Findings (grouped by check category, ordered by severity).

### 5. Deliver

**Default: write the report to a markdown file** at the root of the PRD's containing folder, named `prd-checker-report.md` (or `<prd-slug>-checker-report.md` if multiple PRDs share the folder). Overwrite any prior report at that path — the latest review is the one that matters. After writing, post a brief summary in chat (gate result + severity counts + the top 3 issues) and link to the file.

Override on request:

- **Inline in chat only** — if the PM explicitly asks for an inline report, or if there's no obvious folder to save into (e.g. PRD was pasted into chat with no file path). Skip the file write and put the full report in the response.
- **Annotated copy** — if the PM asks for findings inserted as inline comments in the original document, do that instead of (or in addition to) the standalone report file.

After delivering, suggest next steps based on the gate result:

- **If issues were found:** Take the report back to **prd-builder** in update mode to fix the flagged items, then re-check. The builder is where the PRD gets fixed — the checker only diagnoses.
- **If the gate passed:** Suggest the next step in the pipeline:
  - *(Optional)* **prd-prototyper** — build a clickable prototype for stakeholder validation. The prototype travels with the PRD so engineering gets both.
  - Then **prd-decomposer** to break into sequenced epics with SpecKit prompts — or hand the PRD (and prototype, if created) to engineering directly.

## Edge Cases

**Partial/draft PRDs:** If the PRD is clearly a work-in-progress, adjust framing. Still flag missing sections, but frame them as "sections to add" rather than "sections missing." Focus on content quality checks for what's there.

**Non-standard formats:** If the PRD doesn't follow the reference template, adapt. Map existing sections to the closest template equivalent, run the same checks, and note structural differences.

**Very short PRDs:** A one-pager or brief spec may intentionally skip sections. Drop missing-section flags to 🔵 Info level and focus on content quality — ambiguity, conflicts, traceability.

**Multiple PRDs:** Process one at a time with separate reports. Don't cross-reference between documents unless asked.

## Reference Files

- `references/check-rules.md` — Complete ruleset for all 10 check categories with severity guidance and examples
- `references/report-template.md` — Exact format for the summary dashboard and detailed findings
