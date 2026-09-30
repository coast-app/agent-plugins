# coast-context

Product concepts and modeling guidance for agents designing and using Coast workflows. Paired with Coast MCP access, it helps customer agents turn operational needs into useful configurations and work with existing data.

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-context@coast
/reload-plugins
```

Installing [`coast-mcp`](../coast-mcp) pulls this in automatically.

## Use

The `coast-product-context` skill helps agents choose Coast building blocks for a customer's process:

- [Find the organization's workspaces, access, and shared configuration](skills/coast-product-context/references/product-model.md), and [translate Coast terminology](skills/coast-product-context/references/glossary.md).
- [Choose a model for the process](skills/coast-product-context/references/modeling-decisions.md), then [understand the templates, records, fields, relationships, and subforms](skills/coast-product-context/references/workflow-building-blocks.md) that give it shape.
- [Design collections, forms, and dashboards](skills/coast-product-context/references/views-forms-and-dashboards.md) for the people doing the work.
- [Separate automations from recurring records](skills/coast-product-context/references/automations-and-recurrence.md) when planning what happens next.

Adapt the examples to the customer's process and existing configuration. For step-by-step configuration examples, use [Coast Workspace Patterns](../coast-workspace-patterns/). For available operations and exact parameters, read the connected Coast MCP tool schemas.
