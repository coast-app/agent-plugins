---
name: coast-product-context
description: "Coast product behavior and workflow modeling. Use for components, views, sharing, access, search, notifications, dashboards, AI, data exchange, billing, client capabilities, or configuring and using Coast through MCP."
---

# Coast Product Context

Use this reference to understand Coast and model work with its configurable building blocks. Start with [The shape of Coast](references/product-model.md) for the product hierarchy, [Modeling decisions](references/modeling-decisions.md) to translate a process into a design, or [Terminology](references/glossary.md) for an unfamiliar name. Read only the topics needed for the task.

## Find the owning topic

The [product reference index](references/ontology.md) follows the organization into its workspaces, structured work, and connected experiences.

### The organization and its workspaces

[People](references/organization/people.md), [Access](references/organization/access.md), and [Plans and billing](references/organization/plans-and-billing.md) explain identity and the different limits on an experience. [Workspaces](references/organization/workspaces.md) explains sections, workspace varieties, and lifecycle; [Conversations](references/organization/conversations.md) explains workspace, record, and direct discussions.

### Structured work

- **Definitions and data:** [Workflow templates](references/workflows/templates.md), [Components](references/workflows/components.md), and [Records](references/workflows/records.md). Components includes shared settings, the complete catalogue, relationships, and embedded subforms.
- **Presentation and selection:** [View templates](references/workflows/view-templates.md) owns forms, collection varieties, layouts, and saved versus temporary configuration. [Record selection](references/workflows/record-selection.md) owns filters, groups, sorting, and summaries.
- **Behavior:** [Automations](references/workflows/automations.md) and [Recurring work](references/workflows/recurrence.md).

### Across workspaces

[Search and navigation](references/across-workspaces/search-and-navigation.md), [Attention and activity](references/across-workspaces/attention-and-activity.md), [Dashboards](references/across-workspaces/dashboards.md), and [Reports](references/across-workspaces/reports.md) explain discovery, personal attention, and summaries of work.

### Distribution, exchange, and assistance

[Workflow distribution](references/exchange/workflow-distribution.md) explains the library, bundles, customer sharing, and copies. [Import](references/exchange/import.md), [Export](references/exchange/export.md), and [Integrations](references/exchange/integrations.md) explain external data exchange. [AI assistance](references/ai-assistance.md) explains Ask, Capture, proposals, and saved effects.

## Apply the model

Inspect the customer's existing configuration before proposing a new shape. Use the real business noun when it is clearer: work order, asset, inspection, location, or part. Favor composable capabilities over special behavior inferred from a name. Preserve customer-authored language and the selected view's configuration.

Distinguish product behavior, the person's access, customer configuration, plan limits, client support, and connected-tool capabilities. A schema type alone does not establish an available feature, and missing documentation does not establish absence. Use confirmed account information for current entitlements, prices, and limits.

For construction procedures and worked examples, use `coast-workspace-patterns:coast-workspace-patterns`. For connection, discovery, identifiers, and entity value shapes, use `coast-mcp:coast-mcp-basics`. The connected tools' schemas own available operations and exact inputs. A design can require configuration beyond those tools; identify any remaining setup rather than promising an unsupported operation.
