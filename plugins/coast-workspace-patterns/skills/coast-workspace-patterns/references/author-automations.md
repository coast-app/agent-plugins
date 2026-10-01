# Author an automation

## What Automations Do

Automations combine a trigger, optional conditions, and actions. Use one when a workflow needs an event-driven effect such as a cross-record write, calculated result, or notification. A Related Card or Lookup alone does not write a related record; other operations and configured component behaviors can write without an automation. Use `coast-context:coast-product-context` for the product meaning of relationships and automation behavior.

## Creating an Automation

**`create_automation`** creates the automation, links it to the workflow template, and enables it — all in one call. `enabled` defaults to `true`; pass `enabled: false` only to create it inactive.

Required: `workflowTemplateId` — the workspace's copied template ID, NOT the original source template and NOT the workspace ID.

Before creating a rule, inspect existing automations to avoid a duplicate. Check the connected server's available tools for update, enable, and delete operations. If an update operation is unavailable, plan a safe replacement before creating a corrected rule. Enable a new rule when it should run now; leave it disabled while testing or awaiting activation.

## Trigger Types

| Trigger | When It Fires | Use For |
|---|---|---|
| `ENTITY_CREATED` | Once, when a new entity is created in this workspace | Bridge automations, initial notifications, writing an initial value after creation |
| `ENTITY_UPDATED` | Each time any field on an entity changes | Status sync, conditional actions on field transitions |
| `ADHOC` | When invoked by `trigger_automation`, `TRIGGER_RELATED_CARD_AUTOMATION`, or a configured `SCHEDULED_AUTOMATION` component | Inventory recalculations, scheduled queue changes, and on-demand logic |

The target `ADHOC` automation must be enabled for `TRIGGER_RELATED_CARD_AUTOMATION` to produce its effects. Verify the target's state and a representative run rather than assuming a disabled rule reports an error to the caller.

## Condition Patterns

Conditions gate whether the automation (or a specific action) runs. Each condition compares a left operand to a right operand using a comparison operator. Refer to the MCP tool descriptions for the full list of operand types, comparison operators, and their valid combinations.

### The Transition Detection Pattern (ENTITY_UPDATED)

To fire only when a field *changes to* a value (not every time the entity is saved while already at that value), combine COMPONENT with PREVIOUS_COMPONENT:

```json
{
"conditions": [
  {
    "comparisonOperator": "IS_ALL_OF",
    "leftOperand": { "type": "COMPONENT", "componentId": "<status-tag-component-id>" },
    "rightOperand": { "type": "STRING_ARRAY", "value": ["in-progress"] }
  },
  {
    "logicalOperator": "AND",
    "comparisonOperator": "IS_NONE_OF",
    "leftOperand": { "type": "PREVIOUS_COMPONENT", "componentId": "<status-tag-component-id>" },
    "rightOperand": { "type": "STRING_ARRAY", "value": ["in-progress"] }
  }
]
}
```

This reads: "status is now 'in-progress' AND status was NOT 'in-progress' before this update" — i.e., the entity just transitioned into in-progress.

For a single desired Tag option, `IS_ONE_OF` is also a valid current-value comparison in the connected condition schema. Keep the current and previous checks together; use the operator that expresses the chosen Tag policy.

### The Branching Pattern (Shared Template)

When one template serves multiple entity types (e.g., WOs and PMs), branch automations using a discriminator TAG:

```json
{
"conditions": [
  {
    "comparisonOperator": "IS_ALL_OF",
    "leftOperand": { "type": "COMPONENT", "componentId": "<category-tag-component-id>" },
    "rightOperand": { "type": "STRING_ARRAY", "value": ["pm-category-value"] }
  }
]
}
```

For the non-PM branch, use `IS_NONE_OF` against the PM value.

## Action Patterns

The MCP tool descriptions document the full settings format for each action type. This section focuses on *when* and *why* to use each action, and how they compose.

### UPDATE_CURRENT_WORKFLOW_ENTITY

Updates fields on the entity that triggered the automation. Use for writing an initial value after creation, changing queue visibility, or stamping dates. A form or component default is configured separately and does not require this action by itself.

### UPDATE_WORKFLOW_ENTITY

Updates a field on a RELATED entity in a different workspace. Use for cross-workspace status sync (e.g., WO completion → update Asset operational status). Requires traversing a RELATED_CARD to identify the target entity. Refer to the MCP tool descriptions for the exact settings format — field placement matters here and has specific rules.

### CREATE_WORKFLOW_ENTITY

Creates a new entity in another workspace. Use for auto-generating child records (e.g., WO status change → create Downtime record with timer started). Can copy field values from the triggering entity and link the new entity back to it.

### TRIGGER_RELATED_CARD_AUTOMATION

Fires an ADHOC automation on each entity linked via a RELATED_CARD. Use for cross-workspace recalculation chains (e.g., WO parts change → recalculate Parts Inventory quantities).

**Build order matters:** Create and enable the target ADHOC automation first, then create the source automation that references it by ID.

### CALCULATE_VALUE

Pure computation — produces a number result that must be written to a field by a subsequent action when persistence is needed. The current MCP action schema supports ADD, SUBTRACT, MULTIPLY, and DIVIDE. Read its operand rules before building a chain.

**The `+ 0` idiom.**

The current MCP calculation schema accepts a numeric Component operand directly. An `ADD` with zero can still be useful when a later action needs a named intermediate result, but it is not required just to read Initial Quantity:

```json
{
  "type": "CALCULATE_VALUE",
  "settings": {
    "operator": "ADD",
    "leftOperand": { "type": "COMPONENT", "componentId": "<initial-quantity-component-id>" },
    "rightOperand": { "type": "NUMBER", "value": 0 }
  }
}
```

This produces an action result that later actions can reference by the action's ID. Use the connected tool schema to choose the direct Component operand or a named intermediate result.



**Chaining:** Use placeholder `id` values on source actions and reference them via `actionId` in later actions.

### REFERENCED_IN_QUANTITY_SUM

Summation — produces a numeric action result from matching relationship quantities. It does not by itself persist a Number field; use a later write action when the value must be stored.

### Notification Actions

The configured delivery channels here are email and push. The MCP tool descriptions specify the exact settings format, recipient field requirements, and which recipient sources each action supports. `SEND_NOTIFICATION` may appear as a deprecated email-compatible action name in a connected tool contract; it is not a third delivery channel. The key architectural decision is:

- **PERSON field source** → email or push notification. Email uses `recipientsFromPersonComponentIds`; confirm the connected tool exposes that setting before using it.
- **EMAIL field on the current template** → email notification
- **EMAIL field on a related record** → email (with related-card routing)

### SEND_WEBHOOK

Sends an HTTP POST to an external URL with entity data. Useful for integrating with external systems.

## Compose actions and control effects

### Multi-action rules

One automation can contain multiple actions. If a single business event should create a record, update a related entity, and send a notification, put all three in one `actions[]` array.

Prefer one rule with action-level conditions when several effects share a trigger and policy. Separate rules can be appropriate when they have different owners, lifecycles, or enablement needs. Check condition combination in the current tool contract before assuming a particular `OR` shape.

Put shared trigger logic in top-level `conditions[]`. Use `action.conditions` only when one action has extra gating the others don't share.

### Action-level conditions

Individual actions within an automation can have their own conditions, separate from the automation's top-level conditions. This is useful when one trigger should produce different outputs based on sub-conditions.

For example, a reminder can use one action before the selected Due Date and another after it. Put any shared eligibility rule in the top-level conditions and the date comparisons on the individual actions. The [maintenance example](examples/maintenance-workflow.md#notification-automations) owns its selected date and reminder configuration.

### Suppress an automation chain

`suppressAutomationChain` changes whether resulting writes can trigger further automations. Consider it only after mapping intended downstream effects; suppressing a chain can also skip work the process needs. For a bridge or status sync, test the actual event chain and use stable conditions to prevent loops.

### Example compositions

The [maintenance workflow](examples/maintenance-workflow.md#automations) shows bridge, status-driven, notification, calculation, and cross-workspace trigger compositions. Adapt their events and write owners to the customer process.

## Common Mistakes

| Mistake | What Happens |
|---|---|
| Leaving the target ADHOC automation disabled | Related-record invocation produces no intended effect; inspect the target and test a run. |
| Missing PREVIOUS_COMPONENT in transition detection | Automation fires on every save, not just transitions |
| ENTITY_UPDATED trigger without transition guard | Fires repeatedly when unrelated fields change |
| Creating source TRIGGER_RELATED_CARD before target ADHOC exists | Source references a non-existent automation ID |
| Duplicating rules with the same trigger and conditions | Both may run; inspect existing rules before creating another. |
