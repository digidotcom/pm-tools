# Artifact Guide

Constraints and best practices for building React prototype artifacts in Claude.ai.

## Technical Constraints

Artifacts are single-file React components rendered in the Claude.ai sandbox. Know the boundaries:

**Available:**
- React with hooks (`import { useState, useEffect, useCallback, useMemo } from "react"`)
- Tailwind CSS core utility classes (no custom config, no compiler — only pre-defined classes)
- shadcn/ui components (`import { Button, Card, ... } from '@/components/ui/...'`)
- lucide-react icons (`import { Search, Menu, X, ... } from "lucide-react"`)
- recharts for data visualization (`import { LineChart, BarChart, ... } from "recharts"`)
- d3 for complex visualizations (`import * as d3 from 'd3'`)
- lodash for utilities (`import _ from 'lodash'`)

**Not available:**
- localStorage / sessionStorage — use React state instead
- External API calls from the artifact — mock all data
- React Router — implement navigation with state-based view switching
- Custom fonts — use Tailwind's default font stack
- Separate CSS files — everything is Tailwind utility classes or inline styles

**Key rules:**
- Single file, single default export
- No required props (or provide defaults for all props)
- No `<form>` tags — use onClick/onChange handlers directly
- All data is mocked and embedded in the component

## Structuring a Multi-Screen Prototype

Since React Router isn't available, use state-based navigation:

```jsx
const [currentView, setCurrentView] = useState('dashboard');

// Navigation helper
const navigate = (view) => setCurrentView(view);

// Render based on current view
const renderView = () => {
  switch (currentView) {
    case 'dashboard': return <Dashboard />;
    case 'detail': return <DetailView />;
    case 'settings': return <Settings />;
    default: return <Dashboard />;
  }
};
```

For prototypes with a persistent layout (sidebar, header), render the shell once and swap the content area:

```jsx
return (
  <div className="flex h-screen">
    <Sidebar currentView={currentView} onNavigate={navigate} />
    <main className="flex-1 overflow-auto">
      {renderView()}
    </main>
  </div>
);
```

## Making It Feel Real

**State and interaction:** Use `useState` to make the prototype respond to user actions — filtering a list, opening a modal, toggling a sidebar, selecting a tab, expanding a row. Every interactive element should do something, even if it's just a state toggle.

**Sample data patterns:**

```jsx
// Define sample data at the top of the component
const SAMPLE_DATA = [
  { id: 1, name: "Network Monitor v2", status: "active", owner: "Sarah Chen", progress: 72 },
  { id: 2, name: "Firmware Rollout", status: "pending", owner: "Mike Torres", progress: 0 },
  // ... 6-10 items with varied states
];

// Use state so the data can be "modified" by interactions
const [items, setItems] = useState(SAMPLE_DATA);
```

**Visual polish checklist:**
- Consistent spacing: use Tailwind's spacing scale (p-4, gap-6, space-y-2)
- Typography hierarchy: text-2xl font-bold for page titles, text-lg font-semibold for section headers, text-sm text-gray-500 for metadata
- Color with purpose: use color to indicate status (green/active, yellow/pending, red/error, gray/inactive) — not just decoration
- Cards and containers: rounded-lg border shadow-sm for cards, bg-gray-50 for section backgrounds
- Hover states: hover:bg-gray-100 on clickable rows, hover:text-blue-600 on links
- Empty states: show a meaningful message when a list/table is empty, not just blank space
- Loading states: if the prototype has async-feeling actions (save, submit), show a brief loading spinner via setTimeout

**shadcn/ui components to reach for:**
- `Button` — primary actions, secondary actions, destructive actions
- `Card, CardHeader, CardContent` — content containers
- `Badge` — status indicators, tags, counts
- `Dialog` — confirmation modals, detail views
- `Tabs` — switching between related views
- `Table` — data display
- `Input, Select, Textarea` — form elements
- `DropdownMenu` — action menus, filters
- `Alert` — notifications, warnings
- `Avatar` — user representations

## Size Management

Artifacts have practical size limits. For prototypes:

- **1-3 screens:** Comfortable in a single artifact. Include all screens and navigation.
- **4-6 screens:** Still doable but keep individual screens lean. Extract repeated elements (header, sidebar, common cards) into internal components within the same file.
- **7+ screens:** Pushing it. Consider prototyping only the critical flow, or suggest switching to agent prompt mode.

If a prototype is getting too large, the signals are: the component is over 500 lines, you're copy-pasting similar UI blocks, or the state management is getting tangled. That's when you suggest the agent prompt path.

## Common Prototype Patterns

**Dashboard with cards + table:**
Sidebar navigation, header with user info, grid of summary cards, data table with sorting/filtering. This covers 60% of B2B SaaS prototypes.

**Wizard/multi-step form:**
Progress indicator at top, form fields per step, back/next navigation, summary/confirmation step. Use state for current step and accumulated form data.

**Detail view with tabs:**
Header with entity name/metadata, tabbed content area (overview, activity, settings), action buttons (edit, delete, share). Good for "click a row in the table, see the detail" flows.

**Kanban/board view:**
Columns with cards, drag simulation (click to move between columns since drag-and-drop libraries aren't available), card detail on click. Keep to 3-4 columns max.
