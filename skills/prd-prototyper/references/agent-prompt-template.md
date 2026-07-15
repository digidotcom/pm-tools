# Agent Prompt Template

When the PM chooses agent prompt mode, generate a structured markdown document using this template. The document should contain everything a coding agent needs to build the prototype without asking follow-up questions.

Deliver this as a `.md` file the PM can paste into Claude Code or feed to their coding agent of choice.

## Template

```markdown
# Prototype Build Prompt: [Product/Feature Name]

## What This Is
A clickable UI prototype for [product/feature] — not production code. The goal is [stakeholder demo / UX validation / concept proof / etc.]. All data is mocked. No backend needed.

## Tech Stack
- React + Vite (or Next.js if the PM prefers)
- Tailwind CSS for styling
- [shadcn/ui / Radix / other component library if specified in PRD]
- No backend — all data mocked in-app
- [Any other libraries needed: recharts for charts, dnd-kit for drag-and-drop, etc.]

## Screens

### Screen 1: [Name]
**Route:** /[path]
**Purpose:** [What this screen does and why the user is here]
**Layout:**
- [Describe the layout structure: sidebar + main content, full-width, split panel, etc.]

**Components:**
- [Component name]: [What it shows, what it does when clicked/interacted with]
- [Component name]: [...]
- ...

**Interactions:**
- [Action]: [What happens — navigates to X, opens modal, filters list, etc.]
- [Action]: [...]

**States:**
- Default: [What it looks like on load]
- Empty: [What it looks like with no data]
- Error: [What it looks like when something fails — if applicable]

### Screen 2: [Name]
...

## Navigation
- [How the user moves between screens]
- [Persistent elements: sidebar, header, breadcrumbs]
- [Back/forward behavior]

## Sample Data

[Embed the full sample dataset as a JSON-like structure or JS object]

```javascript
const sampleData = {
  users: [
    { id: 1, name: "Sarah Chen", role: "PM", avatar: "SC" },
    // ...
  ],
  [entities]: [
    { id: 1, name: "...", status: "active", ... },
    // ...
  ]
};
```

Include 6-10 items per entity type with varied states and realistic values.

## Styling Direction
- [Color palette: primary, secondary, accent, status colors]
- [Typography: clean/modern/dense — or reference a design system]
- [Density: spacious dashboard vs. dense data table]
- [Tone: professional/corporate, playful/consumer, technical/developer-facing]
- [Reference screenshots or existing products if applicable: "similar density to Linear" or "Notion-like clean aesthetic"]

## Interaction Details

### [Interaction Name]
- **Trigger:** [What the user does — clicks button, submits form, etc.]
- **Behavior:** [What happens in the UI]
- **Result:** [New state of the UI after the interaction]

### [Interaction Name]
...

## What To Skip
- No authentication / login screens (unless specifically part of the flow being prototyped)
- No real API calls — mock everything
- No deployment config — local dev server is fine
- No tests — this is a prototype
- [Any other explicit exclusions from the PRD's non-goals that apply to the prototype]

## Fidelity Notes
This should look like a real product, not a wireframe. Styled components, realistic data density, hover states, proper spacing and typography. A stakeholder should be able to click through this and react to it as if it were the real thing.

[Any specific design notes from the PRD — mobile responsive, dark mode, specific component library, brand colors, etc.]
```

## Filling the Template

When generating the prompt from a PRD:

**Screens** — derive from the PRD's UX Flows section. Each major view or step in a flow becomes a screen. If the PRD doesn't have explicit screens, infer from Functional Requirements (each feature group often implies a screen).

**Components** — break each screen into its visible elements. Be specific: "a data table with columns for Name, Status, Owner, and Last Updated, with sortable headers and a search input above it" — not "a table showing data."

**Interactions** — every clickable/interactive element needs a defined behavior. If the PRD says "users can filter by status," the prompt should say "clicking a status badge in the filter bar filters the table to show only items with that status. Active filter is visually highlighted. Clicking again removes the filter."

**Sample data** — generate it yourself from the PRD context. Don't leave it as a TODO. The coding agent needs this to build something that looks real.

**Styling direction** — if the PRD specifies a design system, reference it. If not, describe the visual tone in 2-3 sentences. Concrete references help: "clean and minimal like Linear" or "data-dense like Datadog dashboards."

**What to skip** — inherit from the PRD's Non-Goals and add prototype-specific exclusions (no auth, no real APIs, no tests). This prevents the agent from over-building.

## Scope Guidance

The prompt should explicitly state the scope the PM chose:

- **Full app shell:** All screens, navigation, complete flow from entry to exit
- **Critical flow:** Name the specific flow(s). "Prototype only the initiative creation flow: dashboard → new initiative form → confirmation → back to dashboard with the new item."
- **Single screen:** Name the screen. "Prototype only the dashboard view with all its components and interactions."

Being explicit about scope prevents the agent from building more (or less) than intended.
