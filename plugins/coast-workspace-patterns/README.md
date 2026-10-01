# Coast Workspace Patterns

Recipes for designing, building, and revising workspace applications through Coast MCP. Use them to choose a record boundary, connect workspaces, configure a view, or sequence automation actions. Each example is one possible configuration to adapt to the customer's work.

## Install

```text
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-workspace-patterns@coast
/reload-plugins
```

The plugin uses `coast-context@coast` for Coast's product vocabulary and modeling guidance. Claude Code installs that dependency; Codex requires [explicit installation](../../README.md#codex). Connect a Coast MCP server to carry out the recipes; [`coast-mcp`](../coast-mcp) provides connection guidance. The [skill](skills/coast-workspace-patterns/SKILL.md) routes each task to the relevant recipe.

## Use

Start with the product need and inspect the existing organization, workspace, template, and views. Read the matching recipe before writing through MCP. Check the connected tool descriptions for exact inputs and available operations, then inspect the result. The recipes contain construction decisions and selected tool behavior; the connected server owns its current schema.

For product meaning or terminology, use `coast-context:coast-product-context` and its guidance on modeling record boundaries. For MCP discovery and entity value shapes, use `coast-mcp:coast-mcp-basics`. The [maintenance example](skills/coast-workspace-patterns/references/examples/maintenance-workflow.md) shows how several recipes fit together; its fields are one possible configuration, not required Coast defaults.
