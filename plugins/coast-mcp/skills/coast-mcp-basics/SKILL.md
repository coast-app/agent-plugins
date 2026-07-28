---
name: coast-mcp-basics
description: How Coast data is organised — workspaces, workflow templates, workflow entities, and components — and which MCP tools to reach for. Use when working with a Coast workspace through the Coast MCP server, or when a request mentions Coast cards, workspaces, templates, automations, or entities.
---

# Working with Coast through MCP

Coast models operational work as **entities** that live in **workspaces** and take their shape from a
**workflow template**.

## The data model

- **Workspace** — the container, roughly a team's area of work. Everything else is scoped to one.
- **Workflow template** — the schema for a kind of work. It defines the **components** (fields) that
  every entity of that kind carries.
- **Workflow entity** — one record: a work order, an inspection, a task. Sometimes surfaced as a
  "card" in the product and in older API names.
- **Component** — one field on a template. Components are typed (text, number, date, person, tag,
  related card, subform, and others), and each type has its own value shape.

A template belongs to a workspace, and an entity always references the template it was created from.
Read the template before writing entities: component IDs and their allowed values come from there.

## Finding your way around

Start broad and narrow down rather than guessing identifiers:

1. `query_workspaces` to find the workspace.
2. `get_workflow_template` to learn the components, their IDs, and their allowed values.
3. `query_workflow_entities` or `count_workflow_entities` to read entities.
4. `create_workflow_entities` / `update_workflow_entity` to write.

Component IDs are UUIDs, not display labels. Field values are keyed by component ID, so a template
read is a prerequisite for any write.

## Value shapes that commonly trip people up

Reading a template tells you a component's type; the type dictates the value shape. The ones worth
knowing before your first write:

- **TAG** — an array of option *values*, not labels: `["high"]`, not `"High"`.
- **PERSON** — an array of numeric user IDs.
- **NUMBER** — stored in the component's own unit. Currency is integer cents, so $10.00 is `1000`.
  Percent is a decimal fraction, so 15% is `0.15`.
- **RELATED_CARD** — an array of objects, `[{ "id": "<uuid>" }]`, not bare UUID strings.
- **DATE** — an ISO 8601 string. Past dates are valid.

When a write is rejected, re-read the template before retrying; the error usually means a value shape
or an option value does not match what the component defines.

## Reading before writing

Entity writes are not transactional across a batch — if one entity in a `create_workflow_entities`
call is invalid, the whole batch fails. Validate against the template first, and prefer a small batch
you can reason about over a large speculative one.
