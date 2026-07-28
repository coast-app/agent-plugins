# Coast Product Glossary

This reference defines Coast terminology and translates older API-oriented language into current product language.

## Core Terms

| Term | Meaning |
|---|---|
| Organization | The customer/account boundary in user-facing Coast language. |
| Business | The internal/API term for an organization. Users can belong to multiple businesses/organizations. |
| User | A person in a Coast organization/business. |
| User group | A named set of users, commonly used for assignment or permission management. |
| Workspace section | A grouping of related workspaces, used for organization and navigation. |
| Workspace | A shared place for communication and work. Workspaces have chat; workflow workspaces also have structured workflow data. Older API/docs may call this a channel. |
| Workflow workspace | A `CARD_GENERIC` workspace: a workspace backed by a workflow template and cards/workflow entities. |
| Workspace member | A user with access to a workspace. Membership and role settings help determine what the user can view, edit, or administer. |
| Chat thread | The message stream attached to a workspace or card/workflow entity. |
| Workflow | Structured data management inside a workspace. Workflows are powered by workflow templates. |
| Workflow template | The schema for a type of workflow entity. It defines fields/components and how records are structured. |
| Workflow template copy | The active template copy attached to a workspace after installing or creating a workspace. Source/library templates and active workspace templates should not be conflated. |
| Workflow entity | A precise internal/API term for a single record created from a workflow template. |
| Card | The active UI/customer-facing term for a workflow record. Use the specific business noun when possible: work order, asset, request, ticket, location, vendor, part. |
| Component | A typed field or UI element on a workflow template, such as text, date, tag, person, related card, file, subform, static text, or input button. |
| Field | A user-facing value on a card/workflow entity, backed by a component on the workflow template. |
| Related card | The active relationship field from one card/workflow entity to another. |
| Referenced in | The reverse display that shows records that link back through a related-card field. |
| Related card lookup | A field that displays information from a related entity/card without manually duplicating that data. |
| Subform | Embedded structured data inside a parent card, commonly used for procedures, checklists, and inspections. |
| Subform workflow template | The workflow template backing a subform. Subforms are embedded in a parent card but still have a template shape. |
| View template | A saved way to display or collect cards/workflow entities. Views include detail/card views and collection views like list, table, board, and calendar. |
| Card view | A single-entity detail, create, read-only, or form surface. |
| Collection view | A multi-entity surface such as list, table, board, or calendar. |
| External form | A public card view used as a form definition so non-Coast users can submit cards. |
| External form link | A shared distribution link for an external form. Links can support sharing patterns such as QR codes and prefilled values. |
| Automation | A configured rule that reacts to an event or manual trigger and performs actions such as updating fields, sending messages, creating entities, or calculating values. |
| Dashboard widget | A dashboard block that summarizes or links to workflow data, often filtered by status, assignee, dates, or favorites. |
| Dashboard favorite | A saved/favorited dashboard widget or shortcut back to a useful operational view. |
| Workflow bundle | A predefined set of workspace(s), templates, views, dashboards, and automations for a common business scenario. Bundles are starting points that can be customized. |
| Workflow listing | A library/discovery entry for a workflow bundle or installable workflow configuration. Some code/docs may call this a `WorkflowListItem` or bundle listing. |
| Activity feed | A chronological surface for recent activity across Coast. |
| Low Code / LC | The older Coast workflow/card-definition system and migration context. |
| No Code / NC | The newer workflow-template/entity system and builder context. |

## Translation Across Product And API

| Human/Product Term | Internal/API Term |
|---|---|
| Organization | Business |
| Workspace | Channel |
| Card / workflow entity | Card, entity |
| Entity message thread | Thread |
| Workspace section | Workspace section |
| Workflow template | Workflow template |
| View template | View template |

Product terms fit general writing and conversation. API terms fit implementation details, or explanations of why an API uses different naming.

## Common Workspace Types

| Product Concept | Meaning |
|---|---|
| Workflow workspace | A workspace with structured cards/workflow entities, often used for operational processes like work orders, assets, approvals, tickets, or requests. |
| Communication workspace | A workspace primarily used for team chat and collaboration. |
| Direct message | A private one-to-one or small-group conversation. |
| Support direct message | A support-style conversation, exposed in some APIs as `DM_SUPPORT`. |
| Support or external-link workspace | Workspace types that may appear in app/schema contexts, even when not available through every MCP create/list flow. |

## Naming Notes

- "Workspace" is the safest user-facing term for the container.
- Real business nouns are usually clearest when possible: work order, asset, request, ticket, vendor, location.
- "Card" is active UI/customer language for workflow records.
- "Workflow entity" is precise for internal, API, product, and MCP contexts.
- "Template" without context is ambiguous. Disambiguate as workflow template, view template, or subform template.
- "App," "package," "structure," "form/page," and similar terms appear in draft terminology work. Treat them as emerging unless you are working from a current product spec.
