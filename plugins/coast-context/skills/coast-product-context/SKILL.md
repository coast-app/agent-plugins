---
name: coast-product-context
description: Shared Coast terminology and conceptual model for Coast's no-code CMMS and work-management product. Use when tasks reference Coast product concepts such as organizations/businesses, workspaces/channels, cards/workflow entities, workflow templates, components, views, forms, automations, dashboards, or need Coast domain grounding.
---

# Coast Product Context

Use this as domain context for speaking accurately about Coast. Coast is a no-code CMMS and work-management product for maintenance-heavy, deskless, and operational teams. It combines team communication with configurable workflow data: every workspace has chat, and workflow workspaces add structured records built from reusable templates, components, views, relationships, and automations.

This is product context, not a workflow recipe, sales script, implementation guide, or live product-capability matrix. Apply only the facts relevant to the user's task.

## Reference Map

Read only the reference files needed for the task:

- [glossary.md](references/glossary.md): Product vocabulary and legacy/API terminology translation.
- [product-model.md](references/product-model.md): The core hierarchy: organizations/businesses, workspace sections, workspaces, chat, workflow templates, and cards/workflow entities.
- [workflow-building-blocks.md](references/workflow-building-blocks.md): Templates, cards/workflow entities, components, fields, relationships, subforms, and common component categories.
- [views-forms-and-dashboards.md](references/views-forms-and-dashboards.md): Card views, collection views, external forms, layouts, and dashboards.
- [automations-and-behavior.md](references/automations-and-behavior.md): Automations, notifications, scheduled behavior, computed values, and explicit data-writing behavior.
- [modeling-notes.md](references/modeling-notes.md): Product modeling concepts that explain how Coast primitives map to operational work.

## Interpretation Notes

- The real business noun is usually clearest when one exists: work order, asset, location, request, vendor, part, inspection.
- "Card" fits current UI and customer learning material. "Workflow entity" fits internal, product, API, MCP, or implementation contexts where precision matters.
- Legacy/API translations are often relevant: channel means workspace; business usually means organization; card/entity usually means workflow entity.
- Coast is best represented as composable primitives rather than one-off feature modules.
- Preserve the distinction between product concepts and current capability/API mechanics. These references are not a live capability matrix, GraphQL guide, or MCP parameter reference; verify exact behavior against the Coast MCP tool schemas or the product itself before relying on API-level claims.
- For workflow-shaped work, identify the real-world entities, lifecycle, relationships, views, and explicit automations needed to keep data moving.
