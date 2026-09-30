# Coast Product Glossary

Use this reference to translate Coast's product and API terminology. The linked concept references own the detailed behavior.

## Core Terms

| Term | Meaning |
|---|---|
| Organization | The customer/account boundary in user-facing Coast language. |
| Business | The internal/API term for an organization. Users can belong to multiple businesses/organizations. |
| User | A person in a Coast organization/business. |
| User group | A named set of users, commonly used for assignment or permission management. |
| Workspace section | A grouping of related workspaces, used for organization and navigation. |
| Workspace | A shared place for communication and work. Workspaces have chat; workflow workspaces also have structured workflow data. Older API/docs may call this a channel. |
| Workflow workspace | A workspace backed by a workflow template and cards/workflow entities. |
| Workspace member | A user with access to a workspace. Membership and role settings help determine what the user can view, edit, or administer. |
| Chat thread | The message stream attached to a workspace or card/workflow entity. |
| Workflow | Structured data management inside a workspace. Workflows are powered by workflow templates. |
| Workflow template | The schema for a type of workflow entity. It defines fields/components and how records are structured. |
| Workflow template copy | An active workspace's copy of a source/library template. See [template identity](workflow-building-blocks.md#template-sources-and-installed-copies). |
| Workflow entity | A precise internal/API term for a single record created from a workflow template. |
| Card | The active UI/customer-facing term for a workflow record. Use the specific business noun when possible: work order, asset, request, ticket, location, vendor, part. |
| Component | A typed field or UI element on a workflow template, such as text, date, tag, person, related card, file, subform, static text, or input button. |
| Field | A user-facing value on a card/workflow entity, backed by a component on the workflow template. |
| Related card | The active relationship field from one card/workflow entity to another. |
| Referenced in | The reverse display that shows records that link back through a related-card field. |
| Related card lookup | A field that displays information from a related entity/card without manually duplicating that data. |
| Subform | Structured answers embedded inside a parent card; see [Embedded Subforms](workflow-building-blocks.md#embedded-subforms). |
| Subform workflow template | The workflow template backing a subform. Subforms are embedded in a parent card but still have a template shape. |
| View template | Saved configuration for rendering collections and forms; see [Views Are Experience Layers](views-forms-and-dashboards.md#views-are-experience-layers). |
| Card view | A single-entity detail, create, read-only, or form surface. |
| Collection view | A multi-entity surface such as list, table, board, calendar, or tree. |
| External form | A public card view used as a form definition so non-Coast users can submit cards. |
| External form link | A link to a public create form. It can be shared as a QR code or carry prefilled values. |
| Automation | An explicit rule for event, manual, or date-relative actions; see [Automations](automations-and-recurrence.md#automations). |
| Recurring schedule, series, occurrence | A schedule generates or extends separate records; see [Recurring Records](automations-and-recurrence.md#recurring-records). |
| Dashboard widget | A dashboard block that summarizes or links to a saved collection view; see [Dashboards](views-forms-and-dashboards.md#dashboards). |
| Dashboard favorite | A person's server-stored widget shortcut; see [Widget Favorites](views-forms-and-dashboards.md#widget-favorites). |
| Workspace favorite | A client-local shortcut to a workspace; see [Workspace Navigation](product-model.md#workspace-navigation). |
| Dashboard All/Favorites choice | A client-local display choice over widgets; see [Widget Favorites](views-forms-and-dashboards.md#widget-favorites). |
| Workflow bundle | A shareable workspace or section configuration; see [Reusing Workspace Configuration](product-model.md#reusing-workspace-configuration). |
| Library listing | A Coast-curated discovery entry for an installable bundle; see [Reusing Workspace Configuration](product-model.md#reusing-workspace-configuration). |
| Activity feed | A chronological surface for recent activity; see [Conversations And Activity](product-model.md#conversations-and-activity). |

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

## Naming Notes

- "Workspace" is the safest user-facing term for the container.
- Real business nouns are usually clearest when possible: work order, asset, request, ticket, vendor, location.
- "Card" is active UI/customer language for workflow records.
- "Workflow entity" is precise for internal, API, product, and MCP contexts.
- "Template" without context is ambiguous. Disambiguate as workflow template, view template, or subform template.
