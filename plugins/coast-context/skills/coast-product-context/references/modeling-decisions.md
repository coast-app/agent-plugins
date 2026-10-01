# Coast Modeling Decisions

Use this guide to turn an operational process into records, fields, relationships, views, and explicit behavior. Coast combines reusable workspaces, workflow templates, cards, components, view templates, dashboards, and automations; the other references define those pieces. Inspect a customer's existing workspaces and templates before proposing a new shape.

## Start With The Work

Name the things people track, who owns them, how they change, and which questions people return to answer. Work orders, assets, locations, requests, parts, vendors, inspections, and cost entries are possible business nouns, not prescribed Coast modules. Ask:

1. Which things need their own lifecycle, owner, history, or reporting?
2. Which values describe another thing, and which values must remain historical snapshots?
3. Which people need queues, detail forms, schedules, or summaries?
4. Which changes must create, update, calculate, or notify explicitly?

Use the real business noun in customer-facing explanations. [Records](workflows/records.md) and [Components](workflows/components.md) define the available data shapes.

## Choose A Field, Subform, Or Record

Use a field for a value of one record. A finite state or category such as Status, Priority, Stage, Source, Risk Level, or Approval State fits a tag when the option itself needs no lifecycle or detail. A growing list of assets, locations, vendors, customers, employees, or parts usually needs records and related-card fields when people need details, navigation, ownership, reporting, or links from other work. A Person field refers to a Coast user for assignment or approval; it is not a substitute for a domain record such as a Vendor.

Use an [embedded subform](workflows/components.md#subforms-and-embedded-answers) when repeatable answers belong inside one parent card and do not need independent ownership, status, collection views, reporting, or cross-workspace links. Check its component and nesting limits before committing to embedded answers. Use separate related records when each child has its own lifecycle or people must scan, assign, edit, or report on children independently. For example, a checklist within an inspection can be embedded, while separately tracked downtime events or cost entries can relate to a Work Order. A subform is not a child card.

## Choose One Template Or Several

Labels such as request, task, preventive maintenance, and issue do not by themselves require separate templates. One template with a Category or Type field tends to fit when most fields and lifecycle steps are shared, records can change classification, views can filter by type, and automations can branch by it. Separate templates fit when lifecycles, owning teams, fields, and reporting needs differ substantially.

Treat shared-template branching as a design choice, not a promise that a view or tag alone changes behavior. The selected fields and [explicit automation](workflows/automations.md) must support the paths.

## Decide Where A Value Comes From

For each important value, decide whether a person records it, a relationship displays the current value from another card, a form sets a default, or an automation writes a new value. These have different meaning:

| Need | Model |
| --- | --- |
| Preserve what was true when work happened | Store a snapshot on the work record. |
| Show another record's current value | Use a relationship and lookup when available. |
| Start a submission with a value | Set a form/view default, scoped to that entry path. |
| Change a value after an event or calculation | Configure an explicit automation or field behavior. |

An Asset's operational status is live state. A Work Order's copy of that status at the time of a report is a historical snapshot. Duplicating the value can be correct when the distinction is intentional; otherwise a lookup avoids manual drift. A form default does not establish a universal value for every way to create or update a card.

## Choose Relationship Direction And Quantity Ownership

Put an active related-card field where people choose the link. A referenced-in display can show the reverse without maintaining a second link, but it cannot serve as a stored Related Card traversal path for an automation action. Decide which record owns a link and whether the connection itself needs a separately tracked record. See [relationships and lookups](workflows/components.md#connected-records-and-derived-values); the related-record display and traversal recipe in Coast Workspace Patterns explains when a stored path to one current child is needed.

Keep entity quantities, relationship quantities, and derived parent totals distinct. A Part's stock count describes the Part; 3 units used by a Work Order describes their relationship; a total can be aggregated from related usage or cost records. [Relationship quantities](workflows/components.md#quantities-and-relationship-kinds) explains the owners and their updates.

## Design The Surfaces People Need

Field choices determine later presentation: tags support board groups and filters; dates support calendars; numbers support sorting and calculations; relationships support navigation and reuse. [Collection field dependencies](workflows/view-templates.md#collection-types) owns the exact requirements for each view type.

Choose saved collection views for repeatable team questions such as My Open Work, Overdue Requests, triage, status boards, and upcoming schedules. [Collection selection](workflows/view-templates.md#saved-criteria-and-temporary-adjustments) explains where criteria can be saved and how mobile changes behave. A dashboard widget summarizes or links to a saved collection view rather than owning another filter or record set. Choose card views and forms for the information a person should see or enter at a particular step.

Internal control fields for defaults, automation, bridge relationships, computed totals, source tracking, or audit metadata can be hidden, read-only, or placed in a low-priority section when they do not help the person make a decision. Hiding fields is presentation, not permission; [People And Access](organization/access.md) owns the access distinction.

## Choose Automation Or Recurrence

If a record event or manual action should update a value, create another record, send a notification, or calculate a total, configure an automation. If a schedule should generate repeated work, model a recurring series whose occurrences are separate records. A Scheduled Automation component invokes a rule relative to a record's Date field; it does not itself generate a series. A workflow can combine recurrence and a date-relative reminder. See [Automations](workflows/automations.md) and [Recurring work](workflows/recurrence.md).

## Preserve Existing Data Meaning

Before changing a live workflow, inspect existing fields, relationships, views, forms, dashboards, and automation-control values. Archived fields can preserve the meaning of historical card data and allow restoration. Hard deletion can break existing cards or leave values without enough structure to interpret them. Prefer archiving unless current product/tooling guidance establishes that deletion is safe; see [Field Archiving](workflows/components.md#configuration-changes-and-dependencies).
