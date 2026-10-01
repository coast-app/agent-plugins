---
name: coast-mcp-basics
description: "Discover Coast workspaces and templates and read or write entities through MCP. Use for Coast MCP tool selection, identifiers, field value shapes, or rejected entity writes."
---

# Coast MCP Basics

Use `coast-context:coast-product-context` for Coast's product definitions and relationships. This skill
covers discovering data and supplying valid entity fields through MCP. The connected server's tool
schemas own available operations and accepted parameters.

An entity's workflow template defines its components and allowed values. Read that template before
writing fields, using the active template returned for the workspace.

## Finding your way around

Start broad and narrow down rather than guessing identifiers:

1. `query_workspaces` to find the workspace.
2. `get_workflow_template` to learn the components, their IDs, and their allowed values.
3. `query_workflow_entities` or `count_workflow_entities` to read entities.
4. `create_workflow_entities` / `update_workflow_entity` to write.

Component IDs are opaque strings interpreted within their owning template. Use each ID exactly as
`get_workflow_template` returns it; do not infer its format or derive it from a display label.
Entity field values are keyed by those IDs.

## Value shapes that commonly trip people up

Reading a template tells you a component's type; the type dictates the value shape. The ones worth
knowing before your first write:

- **TAG** — an array of option *values*, not labels: `["high"]`, not `"High"`.
- **PERSON** — an array of numeric user IDs.
- **NUMBER** — stored in the component's own unit. Currency is integer cents, so $10.00 is `1000`.
  Percent is a decimal fraction, so 15% is `0.15`.
- **RELATED_CARD** — an array of objects, `[{ "id": "<uuid>" }]`, not bare UUID strings.
- **DATE** — an ISO 8601 string. Past dates are valid.

When a write is rejected, read the error and compare the supplied fields with the template and tool
schema. Refresh the template when a component or option may have changed before retrying.

## Reading before writing

The `create_workflow_entities` contract rejects the batch if an entity is invalid. Validate against
the template first, and use a small batch whose field values have been checked.
