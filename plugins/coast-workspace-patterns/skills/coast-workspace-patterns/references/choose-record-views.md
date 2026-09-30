# Choose collection and card views

View templates shape both saved record collections and Card forms, including public forms. Choose a view for the audience's task, then configure its criteria and component visibility. For product meaning use `coast-context:coast-product-context` and its views and forms guidance; [publish an external request form](publish-an-external-request-form.md) covers submission links.

## Choose the saved view

### Match the view to its task

| Subtype | Best For | Key Feature |
|---|---|---|
| **LIST** | Operational scanning | Focused rows and configured fields; quick actions where supported. |
| **BOARD** | Work organized by a selected group field | The group field and its configured values. |
| **TABLE** | Comparing records across fields | Columns and sorting. |
| **CALENDAR** | Date-based planning | A selected Date field; verify a Date Range's managed endpoint before using it as the calendar anchor. |
| **TREE** | Hierarchical records | A configured parent relationship to the same record kind. |

Card views shape creation and detail forms for a record. Use a separate Card view when a different audience or stage needs a different field set, defaults, or layout. The [maintenance workflow](examples/maintenance-workflow.md#views-and-forms) shows collection and Card views for different audiences.

### Choose the visibility default

| Setting | Effect | When To Use |
|---|---|---|
| `true` | All components hidden unless explicitly shown | A focused List row or form with a small, deliberate field set. |
| `false` | All components visible unless explicitly hidden | A broader Card detail view where only a few fields should be hidden. |

In one work-order configuration, LIST views use `defaultHiddenComponents: true` and explicitly show a small set of fields such as title, date, asset, assignee, and relevant quick actions.

CARD views in that configuration begin with more fields visible and explicitly hide internal or irrelevant fields. Choose the default for the audience and task rather than applying the same visibility rule to every view.

### Choose saved criteria

These example views use standard filter wrappers (refer to MCP tool descriptions for exact syntax):

- **"Assigned to me"** — `containsCurrentUser` on the assignees PERSON field
- **"Overdue"** — `dateIs` with relative date (duration 0, LT = before today) on the chosen Due Date component
- **"Not Complete"** — `containsString` with negate on status TAG
- **Board grouping** — Status Board uses `groupByComponentId` set to the status TAG, with groups for each status value


## Build and verify through MCP

1. Inspect the active copied template and its existing views with `list_view_templates`. Reuse a view that already serves the task, or use `create_view_template` for a new Card or Collection view. Check the connected tool schema for the chosen subtype, its criteria, and component options.
2. Configure `defaultHiddenComponents` and each needed `componentsViewOptions` entry together. For a Collection view, set the saved filter and grouping fields from real component IDs; do not infer them from labels. A Card view can receive a separate [layout](arrange-card-forms.md).
3. Open the view with representative records. Confirm the fields shown, filter results, grouping, and client interaction. A [dashboard entity widget](add-dashboard-widgets.md) counts records from a saved Collection view, so verify that backing population before adding the widget.

The [maintenance example](examples/maintenance-workflow.md#views-and-forms) connects its view inventory to forms, list rows, and dashboards.
