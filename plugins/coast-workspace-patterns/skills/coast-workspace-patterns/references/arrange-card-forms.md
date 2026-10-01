# Arrange fields in a Card form

## What It Does

Card layouts (`create_view_template_layout` / `update_view_template_layout`) control how fields are organized on CARD views — the detail/form views that users interact with when opening or creating an entity. Without a layout, fields render in template component order (top to bottom, one column). With a layout, you can group fields into labeled sections, set multi-column arrangements, and control visual hierarchy.

An explicit layout sets section and component order for the chosen view. Inspect the current view and layout before replacing one; unlisted components may still follow the layout according to that view's visibility configuration.

## Example: different jobs need different layouts

An internal creation Card can put title and problem description first; a detail Card can group history and related work; a public request form can show only submitter fields. The [maintenance workflow](examples/maintenance-workflow.md#card-layouts) contains complete ordered layouts for all three.

## Layout Item Types

### SINGLE_COMPONENT

Renders a field at full width, outside any section. Use for:
- Primary fields that need maximum visual prominence (title, status, notes)
- Full-width fields like FILE, TODO, SUBFORM
- UI components like INPUT_BUTTON
- Fields that don't logically group with others

### SECTION

On a normal workflow template, groups multiple fields under a labeled, collapsible header. Properties:
- `title` — Section label visible to users
- `columnCount` — 1 or 2 columns in the current MCP layout schema
- `componentIds` — Array of component IDs in this section
- `isCollapsed` — Whether the section starts collapsed (default: false)

## Column Count Guidelines

| Column Count | When To Use | Example |
|---|---|---|
| **1 column** | Sequential/temporal fields, list-like content | Schedule (Due Date → Start Date → Reminders) |
| **2 columns** | Paired fields, key-value layouts, compact metadata | Location + Address, Asset + Asset Op Status |

For a subform layout, the connected MCP tool may accept `SINGLE_COMPONENT`, `TEXT`, `IMAGE`, and `VIDEO` items instead of normal-template `SECTION` items. Read the template type and current layout-tool schema before constructing the layout. For text or media items, confirm that the intended form client renders the saved item as expected; tool acceptance alone does not establish support in every editor or client.

## Layout Strategy Rules

### Guideline 1: Put primary fields where the task starts

In a work-order form, title and problem description may deserve the first positions. Other processes may put a location, person, or instruction first. Use `SINGLE_COMPONENT` when a field needs full-width emphasis; a universal Status or File field is not required.

### Guideline 2: Group by the person's task

Don't group "all TAG fields together" or "all RELATED_CARD fields together." Group by what they mean:
- "Location Details" = Location RELATED_CARD + Address LOOKUP
- "Asset Details" = Asset RELATED_CARD + Asset Op Status TAG
- "Time and Cost Tracking" = Duration NUMBER + Cost NUMBER + Parts RELATED_CARD + REFERENCED_INs

### Guideline 3: Match columns to content

Use 2-column sections when fields naturally pair:
- Location + Address (the location and its looked-up address)
- Asset + Asset Op Status (the asset and its current condition)
- Created On + WO ID (metadata pair)

Use 1-column sections when fields are sequential:
- Due Date → Start Date → Reminders (temporal order)
- Procedure → Checklist (workflow order)

### Guideline 4: Use different layouts when jobs differ

Creation, detail, and public submission can warrant different Card views and layouts. Not every Card view needs an explicit layout; use one when the default field order obstructs its task. The [maintenance example](examples/maintenance-workflow.md#card-layouts) shows a full view-by-view choice.

### Guideline 5: Place Referenced In where related work is useful

REFERENCED_IN components are not just passive displays — they also let users **create new child entities directly from the parent card** with the relationship pre-set. For example, a Time Tracking REFERENCED_IN on a Work Order lets the user tap "+" to create a new time entry that's already linked to that WO. This makes placement an important UX decision.

Place REFERENCED_INs based on their domain context, not in a fixed "bottom" position:
- **"Time and Cost Tracking" section** in this example groups Estimated Duration, Estimated Cost, Parts, Costs REFERENCED_IN, and Time Tracking REFERENCED_IN together — because they're all cost/time related and a user working in this section may want to create a new cost or time entry right there
- **Downtime REFERENCED_IN** sits as a standalone component because it's a separate operational concern
- A timer-initiation REFERENCED_IN (like "start tracking wrench time") should be placed near the top or mid-card where the technician actually needs it during work execution, not buried at the bottom

**Rule of thumb:** Place the REFERENCED_IN where the user would naturally want to either view or create the related records. If it initiates a workflow action (like starting a timer), place it prominently. If it's purely historical reference, a lower position or collapsed section is fine.

Place utility controls and metadata according to the intended task; the maintenance example puts several of them below primary work fields.

### Guideline 6: Collapse secondary information when useful

Set `isCollapsed: true` for sections that matter but are rarely consulted, such as detailed accounting fields on an Asset. Test whether collapsing them hides information the person's primary task needs.

## How To Build It

`scaffold_workspace` creates the workspace with a default CARD view but does not take a layout inline — set the layout as a follow-up step against that CARD view.

### Create the initial layout

Use `create_view_template_layout` with the CARD `viewTemplateId`, its `workflowTemplateId`, and an ordered `items` array. On a normal template, use supported `SINGLE_COMPONENT` and `SECTION` items. For a subform template, inspect the connected tool's supported item types before choosing `SINGLE_COMPONENT` or instructional `TEXT`, `IMAGE`, or `VIDEO` items, then open the resulting form in the intended client to check rendering. Get the view ID from `scaffold_workspace` or `list_view_templates`.

### Change an existing layout

Use `update_view_template_layout`, which **fully replaces** the layout. First read it with `get_view_template_layout`; preserve each existing item's ID when retaining it, and pass the complete ordered `items` array. Omitted items are removed and an empty array clears the layout. The update takes `layoutId`, the layout's own ID, not the view-template ID.

## Common Mistakes

| Mistake | What Happens |
|---|---|
| Burying the task's primary input inside a secondary section | People may miss the field they need first. |
| Requesting 3 columns through the current MCP layout tools | Their section schema accepts 1 or 2 columns. |
| Same layout for creation and detail views | Creation forms show too many fields, overwhelming for new entity creation |
| Grouping by component type instead of domain | "All dropdowns" section makes no semantic sense |
| Forgetting that unlisted components still render | Components not in the layout appear after all layout items |

Card layouts are visual. They affect form rendering, not entity creation through the API.
