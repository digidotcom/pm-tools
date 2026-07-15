# PRD Template

Use this structure when building the PRD. Create the document during Step 4 (Draft), then update it incrementally with targeted edits as gaps are filled.

## Template

```markdown
# [Product/Feature Name] PRD

## tl;dr
[2-3 sentence summary — written last, after all other sections are complete]

## Goals

### Business Goals
- [Goal 1]
- [Goal 2]
- [Goal 3]

### User Goals
- [Goal 1]
- [Goal 2]
- [Goal 3]

### Non-Goals
- [Non-goal 1]
- [Non-goal 2]

## User Stories

### [Persona 1]
- As a [user type], I want to [action], so that [benefit].
- ...

### [Persona 2]
- ...

## Functional Requirements

### [Feature Group 1] (Priority: P0)
- **[Feature Name]:** [Description]
- **[Feature Name]:** [Description]

### [Feature Group 2] (Priority: P1)
- ...

## User Experience

### Entry Point & First-Time Experience
- [How users discover/access]
- [Onboarding steps]

### Core Experience
- **Step 1:** [Action]
  - UI Elements: [Components]
  - Validation: [Rules]
  - Navigation: [Transitions]
- **Step 2:** [Action]
  - ...

### Advanced Features & Edge Cases
- [Power user features]
- [Error states]

## Narrative
[200-300 word user story showing problem → solution → outcome]

## Success Metrics

### User-Centric Metrics
- [Metric]: [How measured]

### Business Metrics
- [Metric]: [How measured]

### Technical Metrics
- [Metric]: [How measured]

### Tracking Plan
- [Event 1]: [When triggered]
- [Event 2]: [When triggered]

## Technical Considerations

### UI Architecture
- Framework: [e.g., React with Tailwind]
- Components: [Libraries used]
- Styling: [Approach]

### API & Backend
- Data Fetching: [Approach]
- Authentication: [Method]
- Database: [Solution]

### Performance & Scalability
- [Optimization 1]
- [Scalability consideration]

### Integration Points
- [Dependency 1]
- [Dependency 2]
```

## Section Notes

- **tl;dr** is positioned first for readers but written last during the build. It's a synthesis of everything else.
- **Goals** before **User Stories** because goals frame the "why" that user stories trace back to.
- **Narrative** comes late because by that point the product is fully articulated, making the story easier to write.
- **Functional Requirements** use P0/P1/P2 priority levels. P0 = must-have for launch. P1 = important, can follow fast. P2 = future/nice-to-have.
- **User Experience** should be concrete enough for a designer or coding agent to build from, but shouldn't dictate pixel-level UI. Focus on what the user does, sees, and decides.
- Feature groups in Functional Requirements should map roughly to decomposable epics — if a group can't be built independently, it may need splitting or merging.

## Gap Markers

When drafting from incomplete input, mark gaps inline:

```markdown
### Entry Point & First-Time Experience
- Users access via the main navigation sidebar under "Initiatives"
- [TBD — need: first-time onboarding flow. Is there a tutorial, empty state, or guided setup?]
```

This gives the PM something specific to react to rather than a blank section.
