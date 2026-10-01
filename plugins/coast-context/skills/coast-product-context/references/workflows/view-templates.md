# View templates

A **view template** is a saved configuration for presenting or collecting structured workflow data. The [workflow template](templates.md) defines the components, whether for standalone records or embedded subform answers. A view template chooses how people encounter those components in a card, form, or collection. Several views can show the same work orders for different jobs without creating several copies of the work orders. Subform views similarly present answers held within a parent record.

The view template is itself the saved definition people select. It does not create a separate persistent “view” instance for each person who opens it.

- [The shared view model](#the-shared-view-model)
- [Working with one record](#working-with-one-record)
- [Working with a collection](#working-with-a-collection)
- [Where views are reused](#where-views-are-reused)
- [Managing view templates](#managing-view-templates)

## The shared view model

A view template belongs to a workflow template. Its two fundamental kinds answer different questions:

| Kind | What it configures | Example |
| --- | --- | --- |
| **Card view** | How one record or a set of embedded subform answers is displayed or used as a form. | A detailed Work Order card, an embedded inspection form, or a public request form. |
| **Collection view** | Which records of that kind are selected, how they are ordered or grouped, and how the collection and its cards appear. | An open-work list, a dispatch board, or a maintenance calendar. |

Public access, read-only presentation, create/update use, and collection subtype are properties or uses within these two kinds. See [Components](components.md#definitions-values-and-presentation) for how view overrides apply to field definitions and recorded values.

### Field presentation and form rules

A view can choose which components appear and in what order, whether their labels appear, and how a card or collection presents them. Per-field view options can override applicable component settings, such as a label, placeholder, default, required state, or read-only state. The supported overrides depend on the component type; a view does not invent new component capabilities. [Components](components.md#shared-configuration) owns the common meaning of these settings and of customer-authored text.

The active view supplies the applicable field configuration, including customer-authored labels and placeholders. Hiding a field, making it read-only, and requiring a value are separate decisions. A hidden field can still have stored data, and presentation settings do not replace permissions to read or change a record.

### Selection and defaults

A workflow template can nominate default card views for creating and updating records. The action used to open a card or start a new one may also select a particular view.

## Working with one record

### Card display, creation, and update

A card view presents the components of one record. In a create form, the person supplies values that will become a new record. In an update form, the person changes values of an existing record. The two forms can use different views of the same workflow template: an intake form might ask for the problem and site, while an internal update form also exposes triage and assignment fields.

Defaults and requiredness belong to the selected form experience. The view can supply initial values and applicable field rules; it does not turn those defaults into values on every existing record. A required field in one form is not automatically required in every form for that record kind. A read-only card presentation is another card-view use: it shows an existing record without offering the same editing interaction.

Record writes that do not select a view skip view-based requiredness checks. A field required on a form therefore does not establish that every integration write must supply it. Updates check the resulting record values, including existing values retained by the update. In that view-based write validation, hiding a field does not cancel its requiredness. Hidden or read-only presentation alone does not reject a backend write; validation and authorization remain separate rules.

### Layouts and instructions

A card view can have a saved layout that arranges components in sections and places the record's discussion in the experience. A section can have a title, columns, and a collapsed state. The layout decides where information appears; the component definition and record value determine what the field means and contains.

Put the title and current status where people can find them quickly, group related short fields, and give long notes and files enough space. A role-specific card view can hide or collapse finance, metadata, or automation controls that are not part of that person's task. A complete template can still be hard to use when its card layout is one long list.

For example, a safety inspection form can put equipment details first and the inspection answers in later sections. A subform can supply guidance through an [Info Text](components.md#info-text) component. In configurations that offer subform layout instructions, text, image, and video items can appear among the questions without becoming recorded answers. The web builder offers these instruction items only in its subform editor when that option is available; their presence is not a general card-form capability.

### Embedded subform presentation

A Subform component chooses an allowed subform definition and keeps the filled answers inside its parent record. View templates also configure the create and update presentation of those embedded answers: which questions appear, their applicable overrides, and their layout. The selected subform definition, the parent record's filled value, and the view used to present it are separate things.

The view operates on parent-owned answers. [Subform](components.md#subform) explains the embedded value and question types.

### Public forms and shared cards

An **external form** is a public card-view experience for creating a record. The form's view decides which fields the outside submitter encounters and what its configured copy and defaults say. A separate external-form link distributes access to that experience; opening or sharing the link is not itself a submitted record. A link can also carry initial field values, alongside defaults configured in the view or components.

In the public web form, a link-supplied starting value takes precedence, including an explicit clear. Otherwise, a non-null view default takes precedence over the component default. Relationship prefilling is a separate step that can add related selections afterward.

A **shared-card link** instead points to one existing record and can use a public read-only card view to present it. Submission and read-only disclosure answer different needs: a customer may submit a repair request through the first and inspect its status through the second. A public view's field visibility describes the intended presentation; access to the link and the permission boundary are separate [identity and access](../organization/access.md) questions.

For an external work-order request, a public form might ask for the issue, photos, requester contact details, and location while omitting internal status, assignee, cost, and completion fields. Defaults can supply an intake source or starting status; a configured automation can route the submitted work. Check the actual public form and a representative submission from an outside person's perspective, including any user lists or private fields that could appear. The form link can be shared directly, printed as a QR code, or carry prefilled values.

## Working with a collection

A collection view presents many records of one workflow template. It carries both a **selection** and a **presentation**: saved filters can choose the record population, saved groups can divide it, saved sorts can order it, and the subtype decides how the result appears. Its field options also affect the cards or rows shown within the collection. A view of “Open work” changes which work orders are in focus, not which work orders exist.

### Collection types

| Type | What it helps people see | Configuration it relies on |
| --- | --- | --- |
| **List** | A readable sequence of cards, including an optional checklist-style tag interaction. | Visible fields and ordering; a suitable Tag field when checklist presentation is chosen. |
| **Table** | Records compared across columns. | Component choices for visible columns and any saved sort. |
| **Board** | Records organized into visible groups for work such as dispatch or status review. | Meaningful saved group criteria and, where used, custom card order. |
| **Calendar** | Records placed by a date. | A Date component selected for calendar placement. |
| **Tree** | Records shown through a within-template parent relationship. | A self-referencing Related Card component selected as the tree parent field. |

The web builder offers these five choices and requires a date field for calendar selection and a tree-parent field before creating a tree view. Client availability and interaction details vary by platform.

A Date Range's managed start or end Date can serve as the calendar field where the configuration offers it; the Date Range container itself is not the selected Date. A useful dashboard likewise depends on saved collection views with fields for the operational questions people ask, such as status, assignee, or due date. [Dashboards](../across-workspaces/dashboards.md) owns their widgets and shortcuts.

### Filters, groups, and sorting

Collection criteria have three related jobs:

| Configuration | Effect on the collection | Example |
| --- | --- | --- |
| Filters | Select eligible records using field conditions, including contextual conditions. | Open work orders assigned to the current person. |
| Groups | Organize records through configured grouping criteria; group filters also affect the records represented. | Work organized by Status, with configured groups for the relevant states. |
| Sorting | Order the matching records using selected fields and direction. | Earlier due dates first. |

Saved groups carry criteria that determine the records represented in them. Sorting controls the order of matching records. See [Shared record selection](record-selection.md) for operator rules and supported relationship paths.

### Saved criteria and temporary adjustments

Web allows people with configuration access to create and save collection filters, groups, and sorting. The saved criteria are shared with everyone using that view. Mobile renders those saved configurations and allows temporary adjustments, but cannot save the adjustments back to the shared view template.

Unsaved filters, groups, sorts, and search change the person's working selection. A client can remember those choices across visits without changing the shared definition.

Common saved views include “My Open Work,” “Overdue Requests,” a status board, and a preventive-maintenance calendar. A saved filter defines a working population; it does not expand anyone's access to those records. A collection can also present records read-only without changing the underlying permission to edit them elsewhere.

## Where views are reused

### Dashboard widgets

A dashboard entity widget can point to a collection view and use its saved filters and group criteria to count a matching population, then lead a person back to that view. [Dashboards](../across-workspaces/dashboards.md) describes the widgets and their placement.

### Related-record displays

Referenced In can select a view for presenting the records that point to the current record. For example, an Asset's reverse relationship can show work orders using an appropriate work-order presentation. The relationship determines which records reference the asset; the selected view configures their presentation. [Components](components.md#referenced-in) owns the reverse relationship and its configuration.

### Capture's form context

Capture uses the selected create or update view to determine the fields it can propose into the form. The current draft supplies the values it is working from. This keeps assistance aligned with the form the person is using, including its applicable field rules. Proposing values does not save the record. [AI assistance](../ai-assistance.md) owns proposals, review, and persistence.

## Managing view templates

The web builder creates and maintains view templates. A saved view can be renamed, reordered, copied, or removed. Copying a view provides a separate presentation configuration for the same workflow; the records remain shared.

Selecting a view opens its configured experience. A workflow's [default card views](#selection-and-defaults) supply its create and update experiences, while collection views offer named ways to browse its records. Editing a saved view changes that configuration for its users; [temporary collection adjustments](#saved-criteria-and-temporary-adjustments) affect only the person's working selection.
