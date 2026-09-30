# Add a widget backed by a saved collection

## What Dashboards Do

A Coast dashboard gathers **widgets** that summarize configured record collections and lead people back to the source work. In the entity-widget recipe, the selected collection view determines the record population being counted.

The current `add_dashboard_widgets` tool targets an organization's dashboard. It creates one when its lookup finds none and otherwise appends widgets to the first dashboard returned by that lookup. Do not infer a universal one-dashboard product limit from this tool behavior.

## What a Widget Shows

From the user's perspective, each widget displays:
- The **widget name** (e.g., "Overdue Work Orders")
- The **workspace** it comes from
- A **count** of entities matching the backing view template's filters
- Clicking navigates to that view in that workspace

## Widget Anatomy

Every widget requires exactly four fields:

| Field | Type | Description |
|---|---|---|
| `name` | string | Display label on the dashboard |
| `workspaceId` | number | **Numeric** workspace ID (not UUID slug) |
| `workflowTemplateId` | UUID | The workspace's template ID |
| `viewTemplateId` | UUID | A **COLLECTION** view template with filters |

## Adding widgets with `add_dashboard_widgets`

The `add_dashboard_widgets` recipe has additive semantics. Check the connected server's current dashboard tools before editing an existing widget:

- **Additive, not replacement.** It appends the widgets you pass and keeps existing widgets. Pass only the new widgets; omitting an existing widget does not delete it.
- **Auto-creates the dashboard** if the business doesn't have one yet. No separate create/look-up step.
- **Repeated adds can duplicate widgets.** Inspect the dashboard before retrying a call. Check the connected tool list for other operations instead of assuming this tool is the full dashboard API.

Input shape:

```
add_dashboard_widgets(widgets: [
  { name, workspaceId, workflowTemplateId, viewTemplateId },
  ...
])
```

## The Workflow: Building a Widget

### Step 1: Identify or Create the Backing View

Widgets require a COLLECTION view template. The `add_dashboard_widgets` schema does not restrict the collection subtype to Table, List, or Board. The view's saved criteria determine what gets counted; verify how the selected subtype presents the destination.

If the filtered view already exists, use `list_view_templates` to find it:

```
list_view_templates(workflowTemplateId: "<template-id>", type: "COLLECTION")
```

If no suitable view exists, create one with `create_view_template` first (a COLLECTION view with the filter that defines the count).

### Step 2: Add the Widget

Call `add_dashboard_widgets` with just the new widget(s) — existing widgets are preserved automatically:

```json
{
  "widgets": [
    {
      "name": "Overdue Work Orders",
      "workspaceId": 1234567,
      "workflowTemplateId": "<wo-template-id>",
      "viewTemplateId": "<overdue-view-id>"
    }
  ]
}
```

To seed several widgets at once, pass them all in one call's `widgets` array.

## View Filter Patterns for Widgets

The widget count is driven entirely by the backing view's collection filter. Refer to the MCP tool descriptions (`create_view_template`) for the exact filter wrapper syntax and operators.

The key architectural decisions are about which filter CONCEPTS to combine:

- **TAG match/exclude** — Include or exclude entities by tag value (e.g., status ≠ complete).
- **Relative date comparison** — Filter by the chosen Date field relative to today (e.g., Due Date < today for overdue work).
- **Current user** — Filter to entities assigned to the logged-in user. Creates personal "my queue" widgets.
- **Numeric threshold** — Filter by quantity comparisons (e.g., on-hand ≤ 0 for out-of-stock).
- **Empty check** — Filter to entities where a field is or isn't populated (e.g., unassigned work orders).

Combine the criteria on the backing collection. The [maintenance view inventory](examples/maintenance-workflow.md#work-order-view-set) owns the AND-combined date, visibility, and status criteria for its Overdue Work Orders queue; its widget reuses that collection unchanged.

## Example dashboard

The [maintenance workflow](examples/maintenance-workflow.md#dashboard) connects six widgets to saved collections across work orders, assets, and parts. The count and filters belong to each backing view.

## Building a Widget from Scratch: Full Example

Goal: "Low Stock Parts" widget — count parts where on-hand quantity ≤ 5.

**Step 1: Create the filtered view.** Use `create_view_template` with a COLLECTION/LIST subtype and a numeric filter on the quantity field (≤ 5), selecting the relevant columns (name, quantities, location). This view becomes the widget's backing data source.

**Step 2: Add the widget.** Call `add_dashboard_widgets` with one widget pointing at the view from step 1. The dashboard is created if it doesn't exist, and any existing widgets are kept.

## Common Mistakes

| Mistake | What Happens |
|---|---|
| Using a CARD view template instead of COLLECTION | The entity widget requires a COLLECTION view. |
| Passing existing widgets back into `add_dashboard_widgets` | Duplicates them — the call is additive, not a replacement; pass only new widgets |
| Re-running the same add to "update" a widget | An additive call may create a duplicate; inspect existing widgets and available tools first. |
| `workflowTemplateId` mismatch with workspace | Widget may show wrong data or fail |
| Expecting `add_dashboard_widgets` to reorder, rename, delete, or favorite widgets | This tool only appends; check the connected server for other operations. |
| Assuming widgets don't auto-update when view filters change | They DO auto-update — the widget count always reflects the current view filter results |
