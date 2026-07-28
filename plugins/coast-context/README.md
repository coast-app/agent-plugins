# coast-context

Coast product domain and terminology for agents, so they describe and model Coast work accurately.

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-context@coast
/reload-plugins
```

Installing [`coast-mcp`](../coast-mcp) pulls this in automatically.

## What you get

The `coast-product-context` skill, covering:

| Reference | Contents |
| --- | --- |
| `glossary.md` | Product vocabulary, and how it maps to older API naming |
| `product-model.md` | Organizations, workspace sections, workspaces, chat, templates, entities |
| `workflow-building-blocks.md` | Templates, entities, components, relationships, subforms |
| `views-forms-and-dashboards.md` | Card and collection views, external forms, layouts, dashboards |
| `automations-and-behavior.md` | Automations, notifications, scheduling, computed values |
| `modeling-notes.md` | How Coast primitives map onto real operational work |

References load individually, so only what a task needs enters context.

## Scope

This is product context, not a capability matrix or API reference. It describes how Coast is
organised and what the terms mean. For exact tool behavior, read the Coast MCP tool schemas.
