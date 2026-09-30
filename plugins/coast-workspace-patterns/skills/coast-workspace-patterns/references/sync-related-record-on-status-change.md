# Sync a related record on status change

Use this recipe when a change to one record should create or update a related record, run an automation on it, or notify someone. A Tag change alone does not write a related Asset or tracking record. First decide which event owns the effect: record creation, a transition into a state, or every edit while the state holds. [Author automations](author-automations.md) owns trigger and condition mechanics, including the `PREVIOUS_COMPONENT` transition guard and Tag-array comparisons.

## Choose the related-record effect

| Desired effect | Action and prerequisite |
| --- | --- |
| Update a field on one selected related record | `UPDATE_WORKFLOW_ENTITY`; the source needs a `RELATED_CARD` path to the target. |
| Run a rule on each selected related record | `TRIGGER_RELATED_CARD_AUTOMATION`; create and enable the target `ADHOC` automation first, then reference its ID from the source. |
| Create a new tracking record | `CREATE_WORKFLOW_ENTITY`; map its fields and its link back to the source. |
| Notify an external requester | `SEND_EMAIL` with an Email component; confirm the recipient field and the chosen event. |

A parent `REFERENCED_IN` displays matching children but is not an action traversal path. If a parent-side automation must act on one current child, read [display and traverse related records](related-record-display-and-traversal.md) before adding a stored bridge. That bridge replaces its one selected child when a new child takes over; it is not a collection of all children.

To stop a related record's Timer, use `UPDATE_WORKFLOW_ENTITY` through the source's Related Card path. In the action's `fields[]`, target the Timer component and supply `dynamicData: { "type": "TIME_TRACKER_DATA_ACTION", "dataAction": "STOP" }`. This dynamic field value closes open timer intervals; it is not a separate top-level action. A target `ADHOC` rule can contain the same update when the design calls for invoking a reusable related-record rule. Check the connected action schema and inspect the resulting intervals after a run.

## Build the event chain

1. Inspect the source Tag's allowed option values, related fields, active template ID, and existing automations. Define behavior for an unset or unexpected value.
2. For a transition-only effect, use `ENTITY_UPDATED` with current `COMPONENT` matching the destination and `PREVIOUS_COMPONENT` not matching it. The latter operand exists only for updates. For an effect needed on initial creation too, provide a separate `ENTITY_CREATED` path or an appropriate action-level design; an update rule does not run for initial creation.
3. Choose the narrowest target action from the table. If using a related `ADHOC` rule, create and enable it before the source rule. For multi-action events, decide whether one automation with action-level conditions or separate rules has the clearer ownership and enablement lifecycle.
4. Trace secondary writes and possible loops. Use stable transition guards; set `suppressAutomationChain` only after checking which downstream effects it would skip.
5. Test creation, the target transition, an unrelated edit after the transition, and a repeated save. Inspect both source and target records and any notification recipient.

Use the complete current-and-previous Tag condition in [author automations](author-automations.md#the-transition-detection-pattern-entity_updated) for a transition-only effect. Replace its illustrative option and component ID with values from the active template, and check the connected condition schema before creating the rule.

## Maintenance application

In the [maintenance workflow example](examples/maintenance-workflow.md#automations), a Work Order transition can stop a linked timer, update an Asset's condition, and email an external requester. Those are selected effects, not a universal consequence of reaching Complete. A separate preventive-maintenance branch uses a Category Tag on a shared template; [author automations](author-automations.md) owns the branch conditions, and [configure recurring work](configure-recurring-work.md#configure-the-series) owns recurring defaults.

## Common mistakes

| Mistake | Consequence |
| --- | --- |
| Use `PREVIOUS_COMPONENT` with `ENTITY_CREATED` | No previous state exists; use a creation condition instead. |
| Trigger on every `ENTITY_UPDATED` without a transition guard | Unrelated edits can repeat the effect. |
| Assume a reverse display is a stored action path | The target action cannot traverse `REFERENCED_IN`. |
| Create the source before its target `ADHOC` rule exists or leave the target disabled | The intended related effect does not run. |
| Treat an unset Category as the ordinary branch without checking it | A negative Tag condition may not express the intended fallback. |
