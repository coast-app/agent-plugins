# Build a reusable form inside a record

Use a Subform when a record needs structured answers that remain part of that record, such as an inspection checklist or procedure. The subform template defines reusable questions; the parent record stores the selected subform and its filled answers. Use separate records and Related Card when each item needs its own identity, discussion, discovery, or lifecycle. Use `coast-context:coast-product-context` for the product distinction between embedded answers and separate records.

## Choose the question structure

A parent template can have one `SUBFORM` component. That component can list several subform templates, allowing a person to choose the appropriate procedure, or just one for a fixed form. The server enforces the one-component limit; multiple subform definitions within that component remain possible.

Subform-specific answer types include `AUDIT_TAG`, `AUDIT_TEXT`, and `AUDIT_CHECKBOX`. They can retain supporting notes and files with an answer; `AUDIT_TEXT` and `AUDIT_TAG` stand in for ordinary `TEXT` and `TAG` inside subforms. `STATIC_TEXT` supplies instructions. Other supported input types include Number, Date, Person, Email, URL, File, Address, location, Signature, To-do, and Timer. Subforms cannot contain another `SUBFORM` or relationship fields. Check the connected `create_workflow_template` description for the exact accepted types and required settings.

For example, a work-order Procedure field could offer a routine procedure and a safety procedure. An inspection's Checklist field could offer one fixed definition. These are possible configurations, not mandatory work-order or inspection fields.

## Build and verify through MCP

1. Create each question definition with `create_workflow_template` using `type: "SUBFORM"`. A subform template does not require the standalone template's `name` Text component. Give each new component a UUID from `generate_uuid`, except where the tool explicitly allows a reserved ID.
2. Add one `SUBFORM` component to the parent template and set its `workflowTemplateIds` to the created subform template IDs. Use the active tool description for the component's top-level fields. The component's allowed definitions and the selected definition in a filled value are distinct.
3. If creating a workspace from a source template, inspect the workspace's copied parent template with `get_workflow_template`. Use the subform IDs now listed on its `SUBFORM` component when configuring or writing embedded values. The copy operation can duplicate and remap owned subform templates; do not assume the source IDs survived or create another copy without checking the result.
4. When writing a parent record, set the Subform field inline using one of the template IDs allowed by that copied parent component. Each audit answer uses its type's value wrapper, such as `{ "checked": true }` for `AUDIT_CHECKBOX`. The connected entity tool describes the complete value shape.

If the copied parent has fewer definitions than the source, inspect the duplication result and its compensations before deciding how to restore the intended choices. A duplicate can limit the number of owned subform definitions it copies. Do not silently substitute the original template ID.
