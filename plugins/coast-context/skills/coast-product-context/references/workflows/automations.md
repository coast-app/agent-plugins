# Automations

An **automation** is a rule associated with a [workflow template](templates.md). Its trigger starts an evaluation, conditions decide whether the rule or individual actions proceed, and ordered actions produce effects. A rule can be configured but disabled. A relationship or a calculated result does not itself write another record; the action that makes the change must be configured.

For example, completing a Work Order can update its Asset, a failed inspection can create corrective work, and an external request can notify a triage team. A rule needs an appropriate trigger and one or more actions; optional conditions restrict when the rule or its actions run. A due date, status label, relationship, or form default alone does not perform the write.

## Triggers

The builder offers record-created, record-updated, and invoked-on-demand triggers. Creation and update events can occur repeatedly. A rule meant to respond to a change such as “Status became Approved” therefore needs a condition on that transition, rather than only an update trigger. Invocation is an explicit start of the rule, such as from a related-record automation action or the date-relative path below; it does not mean every record update runs it.

### Date-relative invocation

A [Scheduled Automation component](components.md#scheduled-automation) connects an automation to a record's Date field and configured offsets. It schedules invocation relative to that record's date. Changing the date or the automation can change pending invocations. This is a timing path for an action, distinct from [recurrence](recurrence.md), which creates or extends a series of records. Delivery timing for a particular reminder depends on its configured action and the applicable execution path.

## Conditions and data

Rule conditions gate the whole rule; an action can have its own conditions that gate just that effect. A condition compares selected operands with an operator. Depending on the operand, those values can come from a current or previous field value, a related record, an embedded answer, a literal, a date token, or the output of an earlier action. The available source and comparison depend on the field and action. Distinguish “this record is Approved” from “this record changed to Approved”: the latter needs the previous value as well as the current one.

Actions have an order. A calculation can feed a later update, and conditions on a later action can inspect available earlier results. An output is data available within execution; it is not itself a persisted record value.

## Actions by effect

The web builder exposes 11 action choices:

| Effect | Action choices | What to distinguish |
| --- | --- | --- |
| Write records | Create a record; update a selected record; update the triggering record | Name the target record and fields. Creating a corrective work order is a different write from changing the inspection that triggered the rule. |
| Work through relationships | Update quantity on a Related Card selection; invoke an automation on a related record | Relationship quantity belongs to the selected link, not to a related Part's stock field. Invoking a related rule depends on a configured relationship and that rule's own conditions. |
| Produce values | Calculate a value; sum quantities from records that reference the current record | These actions yield results for later use. A separate update action persists a result in a record field. |
| Deliver or expose | Send email; send a push notification; send a webhook; create an external-form link | A recipient or destination is not a record relationship. A link grants an [external form](view-templates.md#public-forms-and-shared-cards) experience; creating it does not submit a record. |

These are builder choices, not a promise that every plan, permission set, or client supports configuring every action. In-app notifications exist elsewhere in the product; the builder offers email and push actions here. For a concrete rule, confirm the audience, target, and available action in that configuration.

Notification recipients need the right kind of destination. Email can use an address, an Email Address field, or Coast users selected through a Person field; push targets Coast users, often selected through a Person field. A Vendor, Customer, Asset, or Location relationship remains a record link even when the workflow also stores contact information for delivery. An external-form link is a way to invite a submission; creating that link does not create the submitted record.

## Execution, outputs, and change scope

A calculated cost or quantity sum can be passed to a later action. Until an update writes it, the result is not a durable field value. Likewise, changing a work order's Part quantity does not change the Part's inventory. An inventory workflow needs an explicit target write and a policy for edits and repeated events so the same usage is not deducted twice.

Status Tags can also supply board groups, view filters, dashboard counts, and rule conditions. If preventive and reactive Work Orders share most fields and lifecycle steps, a Category Tag can distinguish them and conditions can branch their actions. The category alone does not create different behavior; choose whether one template or several fit the work before configuring those branches.

Actions that create or update records can cause further create or update events, and a rule can explicitly invoke a related rule. Coast checks configured chains for loops, but conditions still need a stable terminal state. A setting can suppress some automatic chaining; it does not turn an action result into a write. When a workflow depends on imports, bulk changes, or recurring generation, establish whether that operation emits the event the rule expects, and whether it suppresses automations. The trigger name alone is insufficient.

When automatic chaining is suppressed, the rule cannot also invoke an automation on a related record. A record update action also skips its write when it would not change any field value. That avoids an update event from an unchanged result; it does not guarantee that repeated real changes are safe. For inventory or other accumulating effects, check how edits and repeated source events are handled.

Automation configuration, revisions, and run history have different roles: the configuration defines future behavior, while history records what was attempted or changed. A successful rule evaluation, a persisted record update, and an external delivery are distinct outcomes. Inspect the specific rule and its execution record when diagnosing one of them.

For the underlying record and relationship ownership, see [Records](records.md) and [Components](components.md). [Import](../exchange/import.md) explains why imported writes need separate effect checks.
