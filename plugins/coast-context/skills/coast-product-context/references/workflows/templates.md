# Workflow templates

A **workflow template** defines a kind of structured work in an organization. A Work Order template says which fields a work order can hold, how a record title is chosen, and which saved [views](view-templates.md) can present or collect its records. A template is a definition, not a work order. The work orders themselves are [records](records.md), also called cards or workflow entities.

A template usually represents a durable business concept rather than one screen. Work Orders, Assets, Locations, and Part Usage can each have a different record shape and lifecycle.

Templates can be reused where the organization does that kind of work. A workspace gives people a place to find records and discuss them; it does not replace the template definition. A template has its own identity even when its name changes or a second workspace uses it. [Workspaces](../organization/workspaces.md) explains the workspace context. A copied or installed template is a new definition with its own identity; [workflow distribution](../exchange/workflow-distribution.md) owns how copies and installation lineage work.

## Define the kind of record

The template carries [component definitions](components.md): a Text field for a description, a Tag field for a configured status, a Related Card field for an Asset, and so on. A component definition supplies the field's identity and capabilities; a record holds its value for that component. A view can change how a component is presented or used in a form without changing the underlying value or creating another field. Component IDs matter when matching a definition to record values, views, and automations. Interpret one within its template, since distinct template copies can retain the same component IDs.

The template's name labels the kind of work, such as “Work Order.” The title of one work order comes from a selected Text component on that record, such as “Repair dock door 4.” The selected title component belongs to template configuration; the text value belongs to the record. Titles are for people to recognize work and need not be unique record identifiers. [Records](records.md#identity-and-content) explains record identity and displayed titles.

Workflow templates can nominate default card views for record creation and update. For example, a short intake form may be the creation default while a fuller internal form is the update default. An opening action can select a particular view instead. Defaults choose an experience; they do not duplicate the record kind or force every interface to use the same form. [View templates](view-templates.md#selection-and-defaults) explains selection and view-specific rules.

## Standalone records and embedded answers

A **standalone template** defines independently managed records. Each record has its own identity, workspace context, values, and attached discussion. Use one when the item should be found, related, or managed on its own: an Asset or a Work Order, for example.

A **subform template** defines reusable questions whose filled answers live inside a parent record's Subform value. The parent template has a Subform component that selects an allowed subform definition. A filled inspection within a Work Order therefore does not become a second work-order-like record, workspace item, or conversation thread. Its definition is reusable, while each filled answer belongs to its parent. See [Subforms and embedded answers](components.md#subforms-and-embedded-answers) and [their form presentation](view-templates.md#embedded-subform-presentation).

This distinction guides modeling. Use a Subform for a procedure whose answers only make sense as part of its parent. Use a standalone template and a [Related Card](components.md#related-card) connection when each inspection or asset needs independent identity, discussion, relationships, or lifecycle. The business name alone does not decide this.

## Change and reuse a definition

Authorized builders can add and configure components, choose the title field and default views, and maintain associated views. Changes to a template affect the definition used by its records; they are not a bulk rewrite of historical field values. Components can be archived without erasing the fact that records previously held their values. Review dependent views, filters, automations, and integrations before changing a field's meaning or removing its availability. [Components](components.md#configuration-changes-and-dependencies) explains field-level dependencies.

Duplicating or installing a template makes a separate template, not a second name for the source. Its new template ID identifies the result. Installation may also create workspace and view copies; names or source lineage alone are insufficient to locate the result of a particular installation. [Workflow distribution](../exchange/workflow-distribution.md) explains identity mapping and reuse across organizations.
