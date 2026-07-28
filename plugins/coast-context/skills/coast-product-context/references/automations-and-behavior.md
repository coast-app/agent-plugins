# Automations And Product Behavior

This reference describes how Coast reacts to changes, sends notifications, creates records, calculates values, and keeps workflow data synchronized.

## Contents

- [Core Automation Model](#core-automation-model)
- [Explicit Data-Writing Rule](#explicit-data-writing-rule)
- [Notifications](#notifications)
- [Scheduled Behavior](#scheduled-behavior)
- [Calculations And Rollups](#calculations-and-rollups)
- [Quantity Patterns](#quantity-patterns)
- [Status-Driven Workflows](#status-driven-workflows)
- [Branching Behavior](#branching-behavior)
- [Guardrails](#guardrails)
- [Audit And Evidence](#audit-and-evidence)

## Core Automation Model

Automations are explicit builder rules configured in a workspace or workflow context. They operate against cards/workflow entities, fields, related cards, schedules, and external actions. An automation watches for an event or manual action, optionally checks conditions, and then performs one or more actions.

Common triggers:

- a card/workflow entity is created
- a card/workflow entity is updated
- a user manually runs an action

Scheduled behavior is usually modeled with date/time-relative configuration and a scheduled automation field/component that causes configured behavior to run at the right time; verify the current tool/API shape before treating it as a separate trigger type.

Common actions:

- update the current card/entity
- create another card/entity
- update a related card/entity
- send an email, push notification, or in-app notification
- trigger another automation through a relationship
- calculate or aggregate a number
- create an external form link
- send a webhook

## Explicit Data-Writing Rule

Coast does not usually write business data implicitly. If a value should change because another thing happened, that behavior normally needs an explicit automation or configured field behavior.

Examples:

- A Work Order status changing to Complete can update an Asset's status, but only if that sync is configured.
- A failed inspection can create a Work Order, but only if an automation creates it.
- A Part's remaining quantity can be calculated from usage, but only if quantity tracking and aggregation are configured.
- External form submissions can start with a special status or source, but only if the form/view applies those defaults.

This makes configured behavior explicit, so agents should not assume data moves by itself.

## Notifications

Automations can notify people when work changes.

Common notification patterns:

- email an external requester when a request is received or completed
- push notify an assignee when urgent work is assigned
- notify a manager when approval is needed
- send a webhook to another system when a status changes

Notification recipients can be literal contacts, Coast users, or values stored in fields, depending on the workflow design.

Recipient type matters. Email notifications need email recipients or email fields; push notifications need Coast users/person fields. A domain relationship such as Vendor, Location, Asset, or Customer should usually remain a related-card relationship even if a separate email or person field is needed for delivery.

## Scheduled Behavior

Scheduled behavior supports reminders, follow-ups, recurring work, and time-relative actions.

Examples:

- remind an assignee before a due date
- trigger a preventive maintenance work order on a schedule
- follow up after a request has been idle
- show recurring work only when it becomes relevant

Scheduled automation commonly pairs with date fields, status fields, and views that separate upcoming, overdue, and completed work.

## Calculations And Rollups

Coast can calculate and aggregate values through configured automations and relationships.

Examples:

- sum child costs into a parent work order total
- compute remaining inventory from initial stock minus consumed parts
- calculate elapsed time or downtime
- update totals when related records change

Distinguish between:

- a value owned by an entity, such as a part's initial quantity
- a value carried by a relationship, such as how many units of a part a work order used
- a value derived from children, such as total cost or total consumed quantity

## Quantity Patterns

Quantity behavior is a common place to confuse data ownership.

- Entity-owned quantities live on the entity. Example: a part has 500 units on hand.
- Relationship-owned quantities live on the relationship. Example: a work order used 3 units of a part.
- Parent totals are usually derived from child records with a rollup/aggregation pattern rather than manually duplicated.
- Cross-field arithmetic generally appears as an explicit calculation plus explicit field update behavior.

This distinction is important for inventory, parts, cost lines, time entries, and other operational tracking.

## Status-Driven Workflows

Status tags often drive automations and views.

Examples:

- Open -> In Progress -> Done
- Draft -> Review -> Approved
- Pending Review -> Accepted -> Completed
- Operational -> Partially Operational -> Non-Operational

Status fields are useful because they can power boards, filters, dashboards, notifications, and branch logic.

## Branching Behavior

One workflow template can support multiple related business paths by using a discriminator field, often a tag.

Example: Work Orders and Preventive Maintenance can share one template when most fields overlap. A Category tag can distinguish PMs from reactive work and drive different automations.

This avoids duplicating data models when records can move between classifications or share most lifecycle behavior.

## Guardrails

Automation behavior has implementation guardrails:

- Loops are possible when one automation updates data that could trigger another automation.
- Bulk/migration operations may or may not trigger automations, depending on current implementation.
- One automation with multiple actions or clear conditions often represents one business event more clearly than several loosely related rules.
- Scheduled, related-card, webhook, and cross-entity updates are implementation-sensitive; current product/tooling behavior should be verified before promising an exact flow.

## Audit And Evidence

Operational workflows often need proof:

- files and photos
- signatures
- geolocation
- timestamps
- user attribution
- checklist results

These fields matter for compliance, inspection, safety, delivery, and field-service contexts.
