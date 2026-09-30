---
name: coast-product-context
description: Coast product concepts and workflow modeling guidance. Use when designing, configuring, or using Coast through MCP; translating an operational process into workspaces, records, components, views, relationships, and automations; or explaining Coast terminology.
---

# Coast Product Context

Use this to reason about Coast and design workflows from its building blocks. Coast is a no-code CMMS and work-management product for maintenance-heavy, deskless, and operational teams. It combines team communication with configurable workflow data: every workspace has chat, and workflow workspaces add structured records built from reusable templates, components, views, relationships, and automations.

Apply the modeling guidance and examples to the user’s process. When acting through MCP, inspect the existing workspace and template configuration and use the connected tools’ schemas for available operations and exact parameters.

## Reference Map

Start with the reader's question and read only the references needed:

| Question | Reference |
| --- | --- |
| Where does work live, and who can access it? | [Product structure](references/product-model.md) |
| What defines a record, its values, relationships, and embedded answers? | [Workflow building blocks](references/workflow-building-blocks.md) |
| How should this operational process be modeled? | [Modeling decisions](references/modeling-decisions.md) |
| How should people browse, enter, and summarize records? | [Views, forms, and dashboards](references/views-forms-and-dashboards.md) |
| What changes a record or generates recurring work? | [Automations and recurrence](references/automations-and-recurrence.md) |
| What does this product or API term mean? | [Glossary](references/glossary.md) |

## Interpretation Notes

- The real business noun is usually clearest when one exists: work order, asset, location, request, vendor, part, inspection.
- "Card" fits current UI and customer learning material. "Workflow entity" fits internal, product, API, MCP, or implementation contexts where precision matters.
- Legacy/API translations are often relevant: channel means workspace; business usually means organization; card/entity usually means workflow entity.
- Coast is best represented as composable primitives rather than one-off feature modules.
- A modeled design may require configuration beyond the connected tools’ capabilities. Establish what those tools support before promising to build it, and identify any remaining setup.
- For workflow-shaped work, identify the real-world entities, lifecycle, relationships, views, and explicit automations needed to keep data moving.
- When a process repeats, distinguish record generation from a rule invoked relative to a Date field.
