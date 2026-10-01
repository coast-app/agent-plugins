# Display related records and traverse a current child

A child record can store a `RELATED_CARD` selection pointing to a parent. A `REFERENCED_IN` component on the parent can display matching children through that forward selection. This reverse display is live; it does not need a second stored link or an automation. For example, an Asset can show Work Orders whose Asset field selects it.

Some parent-side operations need a stored path to a child. An automation action that traverses a `RELATED_CARD` cannot use `REFERENCED_IN` as its traversal field. In that case, a parent `RELATED_CARD` populated by a child-side automation can serve as a bridge to **one intended or current child**. The write in this recipe replaces the parent's field value; a second child would replace the first, even while Referenced In can still display both. Do not use this bridge to represent or traverse all children. That requires a separately supported and verified reconciliation operation or another design. Check current view-filter capabilities separately before adding a bridge solely for filtering.

## Decide whether to add a bridge

| Goal | Configuration |
| --- | --- |
| Show related children on a parent card | Child `RELATED_CARD` to parent plus parent `REFERENCED_IN` referencing that field. |
| Create a child from the parent's reverse display | Configure the `REFERENCED_IN` presentation and verify the intended create flow; no bridge is inherently required. |
| Run a parent automation against one stored current child | Parent `RELATED_CARD` to that child, maintained by an explicit write or automation. |

For a Work Order and a Downtime record, the Downtime record's Service Request field can select the Work Order. A Work Order `REFERENCED_IN` field can then show all matching Downtime records. If an automation triggered by the Work Order must later act on its **current** Downtime record, a second Related Card field on the Work Order can hold that one selection. Define what makes a child current before using this pattern.

## Build the stored path when needed

1. Create the child-side `RELATED_CARD` field targeting the parent template. Use its actual component ID when configuring the parent's `REFERENCED_IN` field. If the reverse display uses a view, select a collection view on the child template.
2. Create a parent-side `RELATED_CARD` field targeting the child template. Hide or make it read-only in relevant views when people should not edit that automation-maintained link. Its value means **the current child**, not the set shown by Referenced In.
3. Configure a child-template `ENTITY_CREATED` automation only if each new child should become the current child. Its `UPDATE_WORKFLOW_ENTITY` action targets the parent through `currentWorkflowEntityComponent.componentId` (the child's parent-link field). In `fields[]`, set `componentId` to the parent's current-child Related Card field and `dynamicData` to `{ "type": "CURRENT_WORKFLOW_ENTITY" }`. Put these properties in the action's `settings`, with `type: "UPDATE_WORKFLOW_ENTITY"` and the target parent's `workflowTemplateId`. That dynamic value is the triggering child as a one-item Related Card selection and **replaces** the parent field.
4. Define how reassignment, deletion, or multiple children affect the current-child choice. A creation-only rule does not reconcile those changes. Test a first and second child, then inspect the parent's stored field and reverse display separately. Confirm the automation is enabled and the target parent exists.

New component IDs passed to MCP authoring tools must be fresh UUIDs from `generate_uuid` (aside from the reserved `name` ID). Component fields such as `workflowTemplateId` and `relatedCardComponentId` are top-level inputs in the current MCP tools, not a nested `config` object. For existing fields, read IDs from `get_workflow_template` and reuse them exactly.
