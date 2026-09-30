# Automations And Recurrence

Coast uses explicit automation rules for event-driven actions and recurring schedules for repeated records. These are distinct ways to move work forward. [Workflow Building Blocks](workflow-building-blocks.md) owns where values live; this reference explains what configured behavior changes them.

## Automations

### Rule Model

Automations are explicit builder rules configured in a workspace or workflow context. They operate against cards/workflow entities, fields, related cards, schedules, and external actions. A rule watches for an event or manual action, optionally checks conditions, then performs one or more actions.

Common triggers:

- a card/workflow entity is created
- a card/workflow entity is updated
- a user manually runs an action

A Scheduled Automation component can invoke a rule at an offset from a record's Date field. This is a date-relative invocation path, distinct from create, update, and manual triggers. It does not generate a recurring series.

Common actions:

- update the current card/entity
- create another card/entity
- update a related card/entity
- send an email or push notification
- trigger another automation through a relationship
- calculate or aggregate a number
- create an external form link
- send a webhook

### Explicit Data Writes

Coast does not usually write business data implicitly. If a value should change because another thing happened, the workflow generally needs an automation or configured field behavior.

Examples:

- A Work Order reaching Complete can update an Asset's status, but only if that sync is configured.
- A failed inspection can create a Work Order, but only if an automation creates it.
- A Part's remaining quantity can be calculated from usage, but only if quantity tracking and aggregation are configured.
- An external form can start a submission with a special status or source only when the selected form/view applies those defaults.

Distinguish an [entered value, live lookup, form default, and automation write](modeling-decisions.md#decide-where-a-value-comes-from) before promising a data flow.

### Notifications

Automations can email an external requester when a request is received or completed, push notify an assignee when urgent work is assigned, notify a manager when approval is needed, or send a webhook when status changes. Recipients can be literal contacts, Coast users, or values stored in fields, depending on the design.

Recipient type matters. Email notifications need email recipients or email fields; push notifications need Coast users/person fields. A domain relationship such as Vendor, Location, Asset, or Customer should remain a related-card relationship even if a separate email or person field is needed for delivery.

### Calculations And Rollups

Configured automations and relationships can calculate and aggregate values: sum child costs into a parent Work Order total, compute remaining inventory from initial stock minus consumed parts, calculate elapsed time or downtime, or update totals when related records change. Cross-field arithmetic generally requires an explicit calculation and field update.

Decide whether a number belongs to an entity, a relationship, or a derived total before configuring the calculation. [Quantity Ownership](workflow-building-blocks.md#relationship-quantities) defines those meanings.

### Status And Branching

Status tags can drive boards, filters, dashboards, notifications, and automation conditions. Example progressions include Open → In Progress → Done; Draft → Review → Approved; Pending Review → Accepted → Completed; and Operational → Partially Operational → Non-Operational.

One template can support related paths when most fields and lifecycle steps overlap. A Category tag can distinguish preventive from reactive Work Orders and branch the rules without creating duplicate data models. The [template decision](modeling-decisions.md#choose-one-template-or-several) comes before the branching configuration.

### Guardrails

- A rule that writes data can trigger another rule, so check for loops.
- Before relying on a bulk or migration operation to trigger rules, check the connected tool's behavior and verify the intended effect on a representative test record.
- One automation with multiple actions or clear conditions often represents one business event more clearly than several loosely related rules.
- For scheduled invocation, related-card updates, webhooks, and cross-entity writes, check the connected tool's available settings and verify the intended effect on a representative test record before promising the flow.

## Recurring Records

A recurring schedule generates or extends a series of records over time. Each occurrence is a separate card/workflow entity with its own values and lifecycle. Changing one occurrence differs from changing the series or stopping future generation. A collection view can show upcoming or overdue occurrences without generating them.

For example, a preventive maintenance schedule can generate Work Orders while a separate date-relative automation reminds the assignee before each occurrence is due. An idle request follow-up may only need a Scheduled Automation rule, with no series. Choose the Date field, schedule or offsets, recipient, and action for each behavior. Current connected tools determine which operations are available.

The distinction between recurrence and date-relative automation also applies when a person says “repeat this work and remind the owner”: model record generation and reminder delivery separately. For configuration recipes, use the Coast Workspace Patterns skill; for exact operations and parameters, inspect the connected Coast MCP tools.
