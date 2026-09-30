# Workflow Building Blocks

This reference describes the data shape of Coast workflows: templates, records, components and their values, relationships, and embedded subforms. For decisions about which shape fits a process, see [Modeling Decisions](modeling-decisions.md).

## Contents

- [Templates And Records](#templates-and-records)
- [Components And Values](#components-and-values)
- [Relationships And Lookups](#relationships-and-lookups)
- [Embedded Subforms](#embedded-subforms)
- [Changing A Live Template](#changing-a-live-template)

## Templates And Records

### Workflow Templates

A workflow template defines the structure for a class of records. If the workspace is "Work Orders," the workflow template defines what a work order contains: title, status, requester, assignee, due date, related asset, notes, files, and so on.

Templates usually map to durable business concepts, not just UI screens. A strong template supports the lifecycle of the real-world thing it represents.

### Workflow Entities

Workflow entities are the records created from a template. They are the live units of work.

Examples:

- a specific repair request in a Work Orders workspace
- a forklift in an Asset Management workspace
- a store in a Locations workspace
- a single cost line item linked to a repair
- a procedure/checklist attached to an inspection

The specific business noun is usually clearer when one exists. "Work order" or "asset" reads better than "workflow entity" in most human-facing contexts.

Standalone cards, such as work orders and assets, have their own discussion threads. Embedded subform answers remain inside the parent card rather than becoming separate child records.

### Template Sources And Installed Copies

Workflow library and bundle templates are starting points. Creating a workflow workspace or installing a bundle makes an active template copy for the resulting workspace. The source/library template says what can be installed; the workspace's copy is what live records, views, automations, and later edits use. Source and installed template IDs identify different templates. Interpret a component ID in the context of its owning template; do not assume every nested ID changes when the template is copied. [Workspace installation](product-model.md#reusing-workspace-configuration) explains bundles and install links.

## Components And Values

### Components And Fields

Component definitions belong to a workflow template; each record stores its own field values keyed by those component IDs. Some components display derived information or provide controls rather than storing a field value of their own.

Common categories:

| Category | Examples | Typical Use |
|---|---|---|
| Text and numbers | text, number, email, URL, address | Names, descriptions, amounts, contact details, links, locations |
| Dates and time | date, date range, scheduled automation, time tracker | Due dates, schedules, reminders, elapsed work, downtime |
| Selection | tag, person | Status, priority, stage, assignee, owner |
| Relationships | related card, referenced in, lookup | Linking records across workspaces |
| Files and evidence | file, signature, geolocation | Attachments, proof of work, onsite confirmation |
| Embedded structure | subform, todo, static text, audit fields | Checklists, procedures, instructions, inspection answers |
| UI helpers | input button, combined tags | Quick actions and dense list displays |
| System or advanced helpers | entity batch, system metadata | Recurrence, batch creation, generated links, auditing |

The Scheduled Automation component invokes a rule relative to a record's Date field; an entity batch can participate in recurrence or batch creation. These are distinct behaviors explained in [Automations And Recurrence](automations-and-recurrence.md).

Component names describe capabilities, not a customer's business meaning. A field labeled “Labor” could hold elapsed time, a numeric estimate, or a link to separate time entries. Choose the value shape first. Preserve customer-authored labels, placeholders, option names, and control text in the active form or view; do not derive placeholder or empty-state wording from a field name. These words explain the expected input but do not change the stored value or field behavior.

### Text, Numbers, And Contact Details

Use Text for a name, description, serial number, or other free-form answer. Short and long text differ in input presentation and configured length; the record title can be a selected Text field. Use Number when the answer is quantitative. Choose its basic, currency, or percentage format to match the meaning of the value, and preserve that format when reading or displaying it: a cost and a rate are not interchangeable bare numbers. Use structured Email Address and Web URL fields for contact addresses and links; use File or Image Upload when the content itself belongs on the record. An image upload is a file field restricted to images.

### Tags

Tags are best for finite states, categories, and process stages.

Good tag examples:

- Status: Open, In Progress, Done
- Priority: Low, Medium, High
- Approval stage: Draft, Review, Approved
- Source: Internal, External

For the choice between a finite category and an independently tracked thing, see [Modeling Decisions](modeling-decisions.md#choose-a-field-subform-or-record).

### Person Fields

Person fields refer to Coast users. They commonly support assignments, ownership, approvals, watchers, or internal recipients.

A person field is different from a domain relationship. For example, a Technician field can assign work to a Coast user, while Vendor, Customer, Location, or Asset records can be linked as relationships. Assignment does not grant workspace access; see [People And Access](product-model.md#people-and-access).

### Calendar Time, Elapsed Time, And Scheduling

Use Date for one point in time, such as a due date or inspection date. Date Range presents a start and end together for a scheduled interval; it is backed by two Date fields. A Timer records elapsed time through start/stop intervals. Several completed intervals can accumulate on one record, and entries can be managed manually when recorded time needs correction. For example, one Work Order's Labor timer can total several visits. If each visit needs its own assignee, approval, rate, discussion, or reporting lifecycle, model separate related time-entry records instead.

These fields do not create business behavior from their labels. A due date does not itself change status; a Timer named “Billable Hours” does not calculate an invoice. Date + Repeat supports recurring record creation, while Scheduled Automation invokes a configured rule relative to a Date field.

### Addresses And Captured Locations

Use Address for a selected physical place, such as a service address. Use Share Location or geolocation when the answer is a device-location observation captured with time and user context. A separately maintained facility or site with its own details and work history is a Location record linked through Related Card; a captured point is not a continuous location history. A work order can need both its service address and proof of where someone completed the work. [Evidence Fields](#evidence-fields) covers other forms of proof.

### Evidence Fields

Operational work may need files and photos, signatures, geolocation, timestamps, user attribution, and checklist results as proof. These fields support compliance, inspection, safety, delivery, and field-service scenarios. Choose the evidence the process actually needs and place it on the record or embedded subform where the event is captured.

### Steps And Reusable Answers

A To-do List keeps editable items and their completion states inside one record. It fits steps people add directly to a card. A Subform fits a reusable procedure or inspection whose configured questions may have different answer types and supporting notes or files. Its filled answers remain embedded in the parent record; it does not create a separate child card. Use related records when individual steps or inspections need their own lifecycle, views, or relationships. [Embedded Subforms](#embedded-subforms) covers their limits.

### Presentation And Controls

Some components help people use or read existing data instead of collecting an independent answer. Combined Tags presents selections from other Tag fields; an input button opens another field's input. Static or info text supplies configured instructions. A two-option Tag can have a checkbox presentation while still storing a selected tag option; a checkbox question inside a Subform is a different answer shape. Choose a control for the underlying answer it acts on, then configure its visible copy for the people using that form.

### System Fields

Coast records also have system-managed metadata such as creator, creation time, update time, sequence number, and links. These are useful for display, sorting, filtering, and auditing, but they are not normal user-authored fields.

## Relationships And Lookups

### Relationships

Relationships connect cards/workflow entities.

The core relationship field is one-way:

- Related card: the active link from one entity to another.

Coast often pairs that with a reverse display:

- Referenced in: a target-side display that shows which other entities link to this one.

Example:

- A Work Order has a related Asset.
- The Asset can show referenced Work Orders.

Relationships make Coast useful for systems of work rather than isolated forms. In high-cardinality relationships, the active related-card field usually sits on the side where users select the relationship, with referenced-in providing reverse visibility instead of a second manually maintained relationship. Referenced In is a display, not a stored path: an automation action requiring a Related Card traversal cannot traverse it. For a parent-side action against one current child, see the related-record display and traversal recipe in Coast Workspace Patterns.

### Relationship Quantities

Some relationships carry a quantity. For example, a Work Order might link to a Part and specify that it used 3 units.

Distinguish three owners:

- An entity-owned quantity describes that entity, such as a part's stock count.
- A relationship-owned quantity describes the link, such as the 3 units of that part used by a work order.
- A parent total can be derived from related child records, such as cost lines or part usage, through configured [calculation and aggregation](automations-and-recurrence.md#calculations-and-rollups).

Cross-field arithmetic and updates require explicit behavior; the data shape alone does not keep values synchronized.

### Lookups

Lookups display information from a related entity. They help users avoid duplicating data manually.

Example: a Work Order can relate to an Asset, and then show the Asset's Location or Serial Number as context.

Lookups fit situations where people need to see related information without manually editing duplicate copies.

## Embedded Subforms

Subforms are embedded structured data inside a parent entity. They are useful for checklists, inspections, procedures, or repeatable sections that should live inside one record rather than as a full separate workspace.

Subforms are backed by subform workflow templates. They can have their own field shape and form behavior, but their data remains embedded in the parent card rather than becoming an independent child card or workspace.

The [subform-or-record decision](modeling-decisions.md#choose-a-field-subform-or-record) explains when embedded answers fit and when separate records need their own lifecycle, views, or relationships.

A parent template can have one Subform component, and that component can offer multiple subform template definitions. Subforms cannot contain nested subforms or relationship fields. These limits matter when choosing embedded answers rather than linked records; use the Coast Workspace Patterns subform recipe and connected MCP tool description for supported question types and setup.

## Changing A Live Template

### Field Archiving

Archived fields are different from hard-deleted fields. Archiving can preserve the semantic meaning of existing card data and make unarchive/restore paths possible. Hard deletion can break existing cards or leave data without enough structure to recover meaning.

Existing workflow changes usually preserve data and field meaning unless a current product/tooling guide explicitly says removal is safe. Removing fields, relationships, view templates, or automation-control fields can affect cards, dashboards, forms, integrations, and training material.

### Check Dependent Surfaces

Before archiving or changing a field, check which saved collections, forms, layouts, dashboards, and automation rules depend on it. For example, changing a Status tag can affect a board's grouping, while changing a Date field can affect a calendar or date-relative automation. [Collection field dependencies](views-forms-and-dashboards.md#field-dependencies) owns view requirements; [Automations And Recurrence](automations-and-recurrence.md) owns behavior.
