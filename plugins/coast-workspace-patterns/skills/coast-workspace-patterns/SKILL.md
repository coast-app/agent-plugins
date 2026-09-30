---
name: coast-workspace-patterns
description: Build and revise Coast workspace applications through the Coast MCP gateway. Use for cross-workspace relationships, conditional automations, recurring work, quantity calculations, public forms, saved views, card layouts, related-record pickers, or dashboard widgets.
---

# Coast Workspace Patterns

Use these recipes to configure a customer's workflow through Coast MCP. Start with the work people need to do and inspect the existing organization, workspaces, templates, views, and automations. Use `coast-context:coast-product-context` for the product meaning of record boundaries, relationships, views, recurrence, and automations; use `coast-mcp:coast-mcp-basics` for discovery and entity value shapes. This skill owns the construction sequence and applied recipes. The connected MCP tool descriptions own current operations and exact parameter schemas.

## Model and build in dependency order

1. Identify record identity and lifecycle, relationships, readers, views, and events that must write data. A field value belongs to one record; a Subform holds embedded answers; a separate related record has its own identity or lifecycle. A shared template fits records with a compatible field contract, while a discriminator Tag can route configured behavior. These are choices, not a required application shape.
2. Inspect existing workspaces and the active **copied** template before editing. An authoring call scoped to a workspace uses its copied `workflowTemplateId`, not the source template or workspace ID. Read current view and automation configuration too, so an addition does not duplicate an existing one.
3. Create reference templates and components before fields that point to them. For a new workspace, inspect `scaffold_workspace`; for template creation, inspect `create_workflow_template`. Their connected schemas own creation parameters. After creating a workspace, read its active template before linking fields. For new component IDs use fresh UUIDs from `generate_uuid`, except the reserved `name` ID where the connected tool specifies it. Reuse existing IDs exactly as returned by `get_workflow_template`. In the current component authoring tools, fields such as `workflowTemplateId` and `relatedCardComponentId` are top-level inputs rather than a nested `config` object; check the active schema for each type.
4. Add the views, layouts, and automations needed by the chosen workflow. A target `ADHOC` automation must exist and be enabled before a source automation refers to its ID. A dashboard entity widget needs a saved collection view first. Read a layout before `update_view_template_layout`: that call replaces the full ordered item array, so retain existing item IDs and include every item to keep.
5. Verify the result in the intended workspace and client. Check a representative record, related selection, view, automation run, or public form as applicable. A saved configuration is not proof that its desired event chain or rendering works.

Distinguish recorded values, live derived displays, form defaults, and automation writes. A Related Card selection can drive a live Referenced In display without another write. Other Coast operations and configured component behaviors can persist values without an automation. Add an automation when the desired write is not already supplied. Visibility, read-only presentation, and picker filters do not replace access or write policy.

## Choose a recipe

**Relationships and embedded answers**

| Need | Recipe |
| --- | --- |
| Show children in reverse, or retain one child for parent-side traversal | [Display and traverse related records](references/related-record-display-and-traversal.md) |
| Narrow a Related Card picker from another field's current value | [Filter related-record pickers](references/filter-related-record-pickers.md) |
| Embed reusable questions and answers in a parent record | [Build subforms](references/build-subforms.md) |

**Behavior and time**

| Need | Recipe |
| --- | --- |
| Choose a trigger, conditions, actions, or a calculation chain | [Author automations](references/author-automations.md) |
| Propagate a status transition to related records | [Sync a related record on status change](references/sync-related-record-on-status-change.md) |
| Generate repeated records and set first/upcoming defaults | [Configure recurring work](references/configure-recurring-work.md#configure-the-series) |
| Reveal upcoming records in an operational queue near their due date | [Configure a recurring queue](references/configure-recurring-work.md#optional-reveal-near-the-due-date) |
| Calculate on-hand quantity from recorded usage | [Track consumed quantities](references/track-consumed-quantities.md) |

**Views, forms, and dashboards**

| Need | Recipe |
| --- | --- |
| Select a saved collection or card view and its filters | [Choose collection and card views](references/choose-record-views.md) |
| Arrange a card or subform's fields for a task | [Arrange card forms](references/arrange-card-forms.md) |
| Accept public submissions or share an existing record read-only | [Public forms and record links](references/publish-an-external-request-form.md) |
| Show several Tags and expose frequent edits on a list | [Show and edit Tags in lists](references/show-and-edit-tags-in-lists.md) |
| Summarize a saved collection on a dashboard | [Add dashboard widgets](references/add-dashboard-widgets.md) |

For a connected example spanning records, relationships, views, automations, and dashboards, read the [maintenance workflow](references/examples/maintenance-workflow.md). Its fields and policies illustrate one configuration, not required Coast defaults.
