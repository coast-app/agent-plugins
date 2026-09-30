# Terminology

Use the customer's business noun when its meaning is known: work order, asset, location, inspection, part, or shift. Use Coast's common model when explaining how those configured records work. An alias helps locate a definition; it does not make every similarly named API type interchangeable.

## Organization and conversation

| Term | Meaning and definition owner |
| --- | --- |
| Organization, business | The customer context for people, workspaces, configuration, and subscription. See [People](organization/people.md). |
| Account, user, membership | A person has an account; membership relates that person to an organization or workspace. See [People](organization/people.md). |
| User group | A named set of organization members that can be associated with workspace access. See [People](organization/people.md#user-groups). |
| Assignee, owner | A configured [Person field](workflows/components.md#person) can select a user; assignment alone is not membership or an access grant. |
| Role, grant, permission | The authority to perform an action in a scope. See [Access](organization/access.md). |
| Workspace, channel | Workspace is the product term. APIs use channel for workspaces and other conversation contexts. See [Workspaces](organization/workspaces.md). |
| Workspace section | An optional grouping for workspace navigation. See [Workspaces](organization/workspaces.md). |
| Workflow workspace, communication workspace | A workspace with structured cards or one used primarily for team conversation. See [Workspaces](organization/workspaces.md). |
| Thread, chat, comments | Human messages occur in workspace discussions, record discussions, and direct conversations. See [Conversations](organization/conversations.md). |
| Unread, seen by, notification, mention, activity | Different signals about content and attention. See [Attention and activity](across-workspaces/attention-and-activity.md). |

## Definitions, records, and fields

| Term | Meaning and definition owner |
| --- | --- |
| Workflow | A configured process using templates, records, views, relationships, and behavior; establish which object the task changes. Start with [The shape of Coast](product-model.md). |
| Workflow template | A definition for standalone records or embedded subform answers. See [Workflow templates](workflows/templates.md). |
| Workflow template copy | The active definition installed or created for a workspace; it can be edited independently of its source. See [Workflow distribution](exchange/workflow-distribution.md). |
| Workflow entity, card, record | One standalone record with its own identity, values, and discussion. Card is customer-facing language; workflow entity is the common API term. See [Records](workflows/records.md). |
| Component, field, value | A component defines a field or interactive element; its value is record data when that component stores data. See [Components](workflows/components.md). |
| Title, name, card number, ID | A visible title, a human-readable number, and an identifier serve different purposes. See [Records](workflows/records.md). |
| Status, priority, category | Usually customer-configured Tag fields, not universal record lifecycle states. See [Tag](workflows/components.md#tag). |
| Related card, referenced in, lookup | Stored forward selection, reverse relationship display, and reading a value through a relationship. See [Connected records](workflows/components.md#connected-records-and-derived-values). |
| Parent, original, source | In a template-copy context these describe provenance, not a parent-child record relationship. See [Workflow distribution](exchange/workflow-distribution.md). A Tree view instead uses a real record relationship to identify a parent; see [collection types](workflows/view-templates.md#collection-types). |
| Subform, procedure | Reusable questions whose answers are embedded in a parent record. See [Subform](workflows/components.md#subform). |
| Checklist | Can mean a To-do List value, a subform procedure, or a collection's completion interaction. Establish the intended experience; see [Components](workflows/components.md) and [View templates](workflows/view-templates.md). |
| Timer, time tracker, duration | Configurable intervals and accumulated time, with their display and editing. See [Timer](workflows/components.md#timer). |
| Quantity, stock | A quantity on a selected relationship is distinct from an inventory value on the related record. See [Related Card](workflows/components.md#related-card). |

## Presentation and behavior

| Term | Meaning and definition owner |
| --- | --- |
| View, view template | Saved presentation/input configuration, not a factory of separate persistent view instances. See [View templates](workflows/view-templates.md). |
| Form | An input experience for creating or updating a record or embedded answers, configured through a view template. See [View templates](workflows/view-templates.md). |
| External form link | A public link to a create form; it can be shared as a QR code or carry prefilled values. See [View templates](workflows/view-templates.md#public-forms-and-shared-cards). |
| Collection, list, table, board, calendar, tree | Presentations of a record population, with different configuration dependencies. See [View templates](workflows/view-templates.md). |
| Filter, group, sort, summary | Shared ways to select, order, partition, and summarize records. See [Record selection](workflows/record-selection.md). |
| Default, override, prefill | Configuration and input sources with different scopes and precedence. See [Components](workflows/components.md) and [View templates](workflows/view-templates.md). |
| Automation, trigger, condition, action | An event-driven rule and its effects. See [Automations](workflows/automations.md). |
| Recurrence, schedule, series, occurrence | Repeated work and the scope of each generated record. See [Recurring work](workflows/recurrence.md). |
| Subscription | May mean a [billing relationship](organization/plans-and-billing.md), a real-time API observation, or a configured component reaction to another value. The latter two uses belong to their API and component contracts; they do not describe billing. |

## Across workspaces and outside Coast

| Term | Meaning and definition owner |
| --- | --- |
| Search | Global discovery, collection search, and picker search have different scopes. See [Search and navigation](across-workspaces/search-and-navigation.md). |
| Favorite, bookmark | Workspace favorites, dashboard widget favorites, and named URL bookmarks have different owners and persistence. A dashboard's All/Favorites choice is a local display setting, not another favorite object. See [Search and navigation](across-workspaces/search-and-navigation.md) and [Dashboards](across-workspaces/dashboards.md). |
| Dashboard, widget, report | Operational summaries of configured collections versus analytical reports. See [Dashboards](across-workspaces/dashboards.md) and [Reports](across-workspaces/reports.md). |
| Library, listing, bundle | A curated discovery entry and the configuration it installs are distinct. See [Workflow distribution](exchange/workflow-distribution.md). |
| Sharing | Can distribute a workflow copy, expose a live record, or collect a new submission. See [Workflow distribution](exchange/workflow-distribution.md) and [View templates](workflows/view-templates.md). |
| Import, export, integration | Ingesting records, producing an external representation, and connecting a provider. See [Import](exchange/import.md), [Export](exchange/export.md), and [Integrations](exchange/integrations.md). |
| App | A Coast client, an integration-provider descriptor, or informal customer vocabulary. In the integration model, see [Integrations](exchange/integrations.md). |
| Ask, Capture, proposal, agent thread, run | AI assistance, form-draft changes, and AI conversation history. See [AI assistance](ai-assistance.md); an agent thread is not a human message thread. |
| Plan, entitlement, limit, overage | Commercial access and measured usage, distinct from member permissions. Entitlement can also name a scoped authorization grant. See [Plans and billing](organization/plans-and-billing.md) and [Access](organization/access.md). |
| Admin | A role, an authorized action, or a settings surface. Identify the object and action, then consult [Access](organization/access.md). |
