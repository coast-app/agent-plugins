# coast-context

Product concepts and modeling guidance for agents designing and using Coast workflows. Paired with Coast MCP access, it helps customer agents turn operational needs into useful configurations and work with existing data.

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-context@coast
/reload-plugins
```

In Claude Code, installing [`coast-mcp`](../coast-mcp) pulls this in automatically. For Codex, follow the [explicit installation steps](../../README.md#codex).

## Use

Use `coast-product-context` to understand existing Coast configuration or design a workflow. Start with [The shape of Coast](skills/coast-product-context/references/product-model.md), then use the [product reference index](skills/coast-product-context/references/ontology.md) to find the relevant concepts and behavior. [Modeling decisions](skills/coast-product-context/references/modeling-decisions.md) helps translate an operational process into records, components, relationships, views, and automations.

Adapt the examples to the customer's process and existing configuration. For step-by-step configuration recipes, use [Coast Workspace Patterns](../coast-workspace-patterns/). For available operations and exact parameters, read the connected Coast MCP tool schemas.
