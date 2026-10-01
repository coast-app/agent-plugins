# Coast product reference

Coast combines team communication with structured operational work. Start with [The shape of Coast](product-model.md) to follow the organization into its workspaces, then see how definitions, records, and presentation fit together. Use the chapters below for the particular product question.

Use these concepts to understand existing configuration and design workflows for the people doing the work. [Terminology](glossary.md) translates customer nouns, overloaded words, and API names into the relevant concepts.

## The organization and its workspaces

### People and access

- [People](organization/people.md): organization and user identity, membership, invitations, and user groups.
- [Access](organization/access.md): organization roles, workspace grants, and effective permission to see, change, or configure work.
- [Plans and billing](organization/plans-and-billing.md): subscription, usage, and commercial access across product experiences.

### Places for work and conversation

- [Workspaces](organization/workspaces.md): optional sections, workspace varieties, conversation and workflow focus, and lifecycle.
- [Conversations](organization/conversations.md): workspace discussion, record comments, direct conversations, and shared message behavior.

## Structured work

### Define the work

- [Workflow templates](workflows/templates.md): record kinds, component definitions, title selection, and default views.
- [Components](workflows/components.md): choose a field by its purpose or name; understand stored values, relationships, derived displays, embedded answers, controls, and managed fields.

### Use the records

- [Records](workflows/records.md): identity, values, displayed titles, lifecycle, history, and attached discussion.
- [Connections between records](workflows/components.md#connected-records-and-derived-values): forward relationships, reverse display, and lookups.

### Present and select the work

- [View templates](workflows/view-templates.md): card and subform forms, collection varieties, layouts, saved configuration, temporary browsing changes, and public card/form access.
- [Record selection](workflows/record-selection.md): filters, groups, sorting, and summaries reused by collections, related-card pickers, and dashboards.

### Configure behavior

- [Automations](workflows/automations.md): triggers, conditions, actions, calculated results, writes, and chaining.
- [Recurring work](workflows/recurrence.md): schedules, occurrences, series defaults, and extension.

## Working across workspaces

- [Search and navigation](across-workspaces/search-and-navigation.md): finding workspaces, people, cards, messages, and comments; returning through favorites and bookmarks.
- [Attention and activity](across-workspaces/attention-and-activity.md): read state, notifications, mentions, and activity feeds.
- [Dashboards](across-workspaces/dashboards.md): widgets that reuse collection views, summaries, and personal favorites.
- [Reports](across-workspaces/reports.md): analytical reports, their scope, and how they differ from operational collections.

## Sharing and exchanging

- [Workflow distribution](exchange/workflow-distribution.md): Coast's curated library, customer sharing, bundles, installation, and independent copies.
- [Import](exchange/import.md): mapping external data into records.
- [Export](exchange/export.md): selected data and external representations.
- [Integrations](exchange/integrations.md): provider connections and their configured exchange behavior.

## AI assistance

[Ask and Capture](ai-assistance.md) explains conversational work assistance, form drafts, proposals, review, and persistence. Their AI conversations are distinct from human workspace and record discussions.

## Apply the reference

For a task, locate the object's definition, then follow its relationships to the affected experiences. A work order, asset, or inspection is a configured use of Coast's primitives; the business noun alone does not establish special behavior. Keep the person's access, customer configuration, plan, and client capabilities distinct.

Use [Modeling decisions](modeling-decisions.md) to choose a configuration, then the Coast Workspace Patterns recipes to build it through supported MCP operations. The connected tool schemas determine available operations and exact inputs. An omission here does not establish that a feature is absent or unfinished.
