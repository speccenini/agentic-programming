# UI Current State Description Template

> Purpose: use this file as a bridge between the developer and an AI coding assistant such as Codex or Opus.  
> First, ask the assistant to inspect the current UI implementation and fill in the **Current State** sections.  
> Then the human reviewer edits the **Requested Changes** and **Acceptance Criteria** sections before asking the assistant to implement the changes.

---

## 1. Screen / Component Identification

### Screen or Component Name
<!-- Example: SAP Systems Overview Page -->


### Route / Entry Point
<!-- Example: /sap-systems -->


### Relevant Files
<!-- List the files that define the UI, layout, styles, components, and data mapping. -->

- `path/to/file`
- `path/to/file`
- `path/to/file`


### Related Components
<!-- Mention child components, shared layout components, drawers, tables, cards, filters, etc. -->

- 
- 
- 

---

## 2. Current UI Purpose

### What This Screen Is Supposed To Do
<!-- Describe the functional purpose of the screen. -->


### Primary User Task
<!-- What should the user be able to do first and most easily? -->


### Secondary User Tasks
<!-- What else can the user do, but with lower priority? -->

- 
- 
- 

---

## 3. Current Layout Description

### Overall Layout Structure
<!-- Describe how the page is currently structured. Example: header, filters, table area, right context panel, footer. -->


### Layout Type Used
<!-- Example: CSS grid, flexbox, fixed-width containers, responsive utility classes, custom CSS. -->


### Width / Height Behavior
<!-- Describe how the layout behaves horizontally and vertically. Mention fixed widths, max-widths, min-widths, overflow, scroll areas. -->


### Current Spacing
<!-- Describe gaps, padding, margins, density, and whether the layout feels too wide, too compressed, or balanced. -->


### Current Responsive Behavior
<!-- Describe behavior on desktop, laptop, tablet, and mobile if visible in the code. -->

- Desktop:
- Laptop:
- Tablet:
- Mobile:

---

## 4. Current Visual Hierarchy

### Main Visual Element
<!-- Which area currently dominates the page visually? Example: table, drawer, empty panel, filters, cards. -->


### Intended Main Visual Element
<!-- Which area should dominate the page based on the purpose of the screen? -->


### Areas That Currently Draw Too Much Attention
<!-- Example: empty drawer, oversized cards, too much whitespace, wide technical columns. -->

- 
- 
- 


### Areas That Currently Need More Emphasis
<!-- Example: table data, attention badges, filters, selected row, warnings. -->

- 
- 
- 

---

## 5. Current Table / Data Presentation

### Table Purpose
<!-- Describe what the table is meant to communicate. -->


### Current Columns
<!-- List visible columns in order. -->

| Column | Current Width / Behavior | Content Type | Issue, If Any |
|---|---|---|---|
| Example: SID | Auto | Short text | None |
| Example: Subscription ID | Auto / very wide | Long technical ID | Too wide |


### Column Priority
<!-- Classify columns by importance. -->

#### Must Be Visible By Default
- 
- 
- 

#### Useful But Can Be Hidden / Collapsed
- 
- 
- 

#### Better Suited For Details Panel / Drawer
- 
- 
- 


### Current Overflow Behavior
<!-- Describe whether horizontal scroll happens at page level or table-container level. -->


### Long Value Handling
<!-- Describe whether long values wrap, truncate, overflow, or expand the layout. -->


### Row Interaction
<!-- Describe what happens when a row is clicked, hovered, selected, expanded, etc. -->

---

## 6. Current Side Panel / Drawer / Context Panel

### Current Role Of The Panel
<!-- Example: details panel, empty placeholder, operations overview, selected system details. -->


### Current Width Behavior
<!-- Include fixed width, percentage width, min/max width, flex behavior, etc. -->


### Empty State Behavior
<!-- What does the panel show when no item is selected? Does it leave empty white space? -->


### Selected State Behavior
<!-- What does the panel show when a row/item is selected? -->


### Problems Observed
<!-- Example: too wide, too empty, forces table compression, long values break panel width. -->

- 
- 
- 

---

## 7. Current Filters / Search / Controls

### Current Controls
<!-- List filters, search fields, buttons, column pickers, refresh buttons, export buttons, etc. -->

- 
- 
- 


### Placement
<!-- Where are these controls placed? Are they visually clear? -->


### Problems Observed
<!-- Example: controls take too much space, labels are unclear, filters wrap badly. -->

- 
- 
- 

---

## 8. Current Empty / Loading / Error States

### Initial Load State
<!-- What does the user see when the app/screen opens? -->


### Loading State
<!-- Describe skeletons, spinners, placeholders, disabled buttons, etc. -->


### Empty Data State
<!-- What happens when there are no rows or filters return no results? -->


### Error State
<!-- How are API errors, authorization errors, and data load failures shown? -->


### Problems Observed
- 
- 
- 

---

## 9. Current Styling System

### Styling Approach
<!-- Example: Tailwind, CSS modules, plain CSS, styled-components, Bootstrap, Material UI, custom classes. -->


### Design Tokens / Constants
<!-- Mention colors, spacing variables, breakpoints, typography scale, if present. -->


### Existing Reusable Components
<!-- Example: Card, Badge, Table, Drawer, Button, Alert, Tooltip. -->

- 
- 
- 


### Styling Constraints
<!-- Mention conventions that should be preserved. -->

- 
- 
- 

---

## 10. Current Technical Diagnosis

### Likely Cause Of The Visual Problem
<!-- Ask the coding assistant to infer from the implementation. Example: side panel has fixed width, table cells have no max-width, parent container uses page-level overflow. -->


### Files / Classes / Components Most Likely Involved

- `path/to/file`
- `className-or-css-selector`
- `componentName`


### Risks If Changed Incorrectly
<!-- Example: mobile layout may break, drawer may overlap table, long values may become unreadable. -->

- 
- 
- 

---

## 11. Requested Changes

> Human reviewer should edit this section after the current state has been described.

### Change Summary
<!-- Example: Make the table the dominant area and reduce the side panel width on desktop. -->


### Desired Visual Outcome
<!-- Describe the expected visual result in plain language. -->


### Layout Contract
<!-- Use concrete numbers where possible. -->

#### Desktop
- Main content / table area:
- Side panel / drawer width:
- Side panel max width:
- Gap between areas:
- Page-level horizontal scroll:
- Table-level horizontal scroll:

#### Laptop
- 

#### Tablet
- 

#### Mobile
- 


### Content Priority Rules

#### Always Visible
- 
- 
- 

#### Can Be Truncated
- 
- 
- 

#### Move To Details Panel
- 
- 
- 

#### Hide By Default / Column Picker
- 
- 
- 


### Behavior Rules

#### When No Item Is Selected
- 
- 
- 

#### When An Item Is Selected
- 
- 
- 

#### When Values Are Long
- 
- 
- 

#### When The Screen Is Narrow
- 
- 
- 

---

## 12. What Not To Do

<!-- This section is very important for AI coding assistants. Be explicit. -->

- Do not solve layout problems by randomly reducing font sizes.
- Do not hide important information without adding an alternative way to access it.
- Do not introduce page-level horizontal scrolling.
- Do not make the layout depend on a single hardcoded screen width.
- Do not let long technical strings force containers wider than intended.
- Do not change unrelated business logic.
- Do not change API contracts unless explicitly requested.
- Do not redesign the entire screen if the requested change is local.

---

## 13. Acceptance Criteria

> Human reviewer should edit this section before implementation.

- [ ] The main visual problem described above is fixed.
- [ ] The screen has no unintended page-level horizontal scroll on desktop.
- [ ] Horizontal scroll, if still needed, is contained inside the table area only.
- [ ] The primary user task is visually easier than before.
- [ ] The side panel/drawer does not dominate the page unless explicitly opened or selected.
- [ ] Long technical values are truncated, wrapped, or moved to detail views without breaking layout.
- [ ] Empty states look intentional and useful, not like unused whitespace.
- [ ] Existing data fetching and business logic still work.
- [ ] Existing authorization and backend integration behavior is unchanged.
- [ ] The implementation remains consistent with the existing styling system.

---

## 14. Suggested Implementation Direction

<!-- Optional. The assistant may propose alternatives if the current code suggests a better approach. -->

Potential techniques:

- Use a two-column flex or grid layout.
- Let the main table area use remaining width with `flex: 1` or `minmax(0, 1fr)`.
- Cap the side panel width with `width`, `min-width`, and `max-width`.
- Use `min-width: 0` on flex/grid children to prevent overflow bugs.
- Put `overflow-x: auto` on the table container, not on the full page.
- Add `max-width`, `white-space: nowrap`, `overflow: hidden`, and `text-overflow: ellipsis` for long technical values.
- Use tooltips, copy buttons, or detail panels for full values.
- Use a column picker for secondary columns.
- Use badges for compact status and attention indicators.

---

## 15. Before / After Description

### Before
<!-- Describe the current user impression. -->


### After
<!-- Describe the expected user impression. -->


---

## 16. Screenshot References

<!-- Add screenshots if available. Annotated screenshots are strongly recommended for visual issues. -->

### Current Screenshot
- `docs/ui-briefs/images/current.png`

### Annotated Screenshot
- `docs/ui-briefs/images/annotated.png`

### Target / Inspiration Screenshot
- `docs/ui-briefs/images/target.png`

---

## 17. Prompt To Ask Codex / Opus To Fill This File

Use this prompt first:

```text
Please inspect the current implementation of this screen/component and fill in the "Current State" sections of this Markdown file. Do not implement changes yet. Focus on describing the existing layout, visual hierarchy, table behavior, side panel behavior, overflow behavior, styling approach, and likely technical causes of the visual issues. Be specific and reference the relevant files, components, CSS classes, and layout rules.
```

Then, after the human reviewer edits the requested changes and acceptance criteria, use this prompt:

```text
Please implement only the changes described in the "Requested Changes" section. Respect the "What Not To Do" section and verify the implementation against the "Acceptance Criteria". Keep business logic, API contracts, authentication, authorization, and data fetching behavior unchanged unless explicitly requested.
```
