# Show and edit Tags in list rows

A List row may need both a compact status summary and a quick way to change a frequent Tag. `COMBINED_TAGS` displays several Tag values together; `INPUT_BUTTON` opens a picker for one Tag on the same template. Both are presentation components without their own entity field values. Configure the source Tags, then decide separately which views show the combined display and each button. [Choose collection and card views](choose-record-views.md) owns view subtype, filters, and visibility defaults.

## Decide what belongs on the row

Use `COMBINED_TAGS` when several status or classification Tags are useful while scanning a collection. Include Tags relevant to most records and put them in the desired left-to-right order. A hidden control flag, infrequent metadata, or a very long option set may be better left out. The source Tags remain the editable and filterable fields; the combined strip adds no filter behavior. Collection criteria still target the individual Tag component IDs.

Use an `INPUT_BUTTON` for a Tag that people change frequently while moving through the list. Priority or category may be useful to display yet rarely changed; an automation-managed or internal control field should not receive a user action without a reason. A button's `inputComponentId` must refer to a Tag on the same template. Multiple buttons can serve different tasks, but test the row width and the intended client's interaction before adding more.

The [maintenance example](examples/maintenance-workflow.md#list-rows) has one combined strip for Asset condition, Work Order status, Category, and Priority, plus two buttons for frequent Status and Asset condition changes. It includes the detailed per-view visibility matrix. Other workflows may choose a different set or use a Board interaction instead.

## Configure the views

1. Inspect the source Tags and add a `COMBINED_TAGS` component with their IDs in the intended display order. Use the connected component tool's exact configuration key; the order controls rendering order.
2. Add an `INPUT_BUTTON` with a user-facing label and `inputComponentId` pointing to each chosen target Tag. Check the current tool schema for its full settings.
3. In a compact List, show the combined display and chosen buttons. Hide individual Tag chips that would duplicate the strip. With `defaultHiddenComponents: true`, explicitly show each presentation component; with `false`, explicitly hide any unwanted ones.
4. In an editable Card, show the individual Tag fields. Hide the combined strip if it repeats the same information. A public request Card can hide internal Tags and buttons entirely. Board, Table, and Calendar views can make different choices based on their native interactions and audience.
5. Open each intended view in the target client. Confirm the combined order, label, row width, and that a button edits the intended Tag. Check a list filter separately; the display component does not change which records appear.

An illustrative List row is:

```text
[Fix HVAC Unit] [Operational | Open | Mechanical | High] [Update Status ▾]
```

## Common mistakes

| Mistake | Consequence |
| --- | --- |
| Show the combined strip and the same individual Tag chips | Duplicates information and crowds the row. |
| Filter on `COMBINED_TAGS` rather than source Tags | It is presentational; use the individual Tag fields in `collectionCriteria`. |
| Include hidden or internal control Tags in the strip | Exposes noise or internal state to the intended reader. |
| Set `inputComponentId` to a non-Tag field | The component authoring call can reject it. |
| Add a button but leave it hidden in the relevant List | The quick action is unavailable where people need it. |
| Repeat a Board's native grouped-Tag edit with an equivalent button | The control may add clutter; check the actual Board interaction. |
| Assume the strip always needs a button | A read-only scan view can intentionally show status without offering edits. |
