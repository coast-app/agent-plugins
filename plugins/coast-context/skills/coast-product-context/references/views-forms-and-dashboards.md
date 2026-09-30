# Views, Forms, And Dashboards

This reference describes how view templates render collections and forms over workflow records, and how dashboards reuse saved collection views. For access controls, see [People And Access](product-model.md#people-and-access).

## Contents

- [Views Are Experience Layers](#views-are-experience-layers)
- [Collection Views](#collection-views)
- [Card Views And Forms](#card-views-and-forms)
- [Dashboards](#dashboards)

## Views Are Experience Layers

View templates are saved configurations for rendering collections of cards/workflow entities and for displaying, editing, or collecting data through forms. Card forms work with a record; subform create/update forms edit [embedded data within a parent record](workflow-building-blocks.md#embedded-subforms). Selecting a view template does not create a separate persistent view instance or a copy of the data. The same record can appear in a list, table, board, calendar, tree, card/detail view, or public shared-card surface. An external form collects a new record. Dashboards sit one level up: dashboard widgets summarize or link to saved collection views rather than acting as another record view.

This means a workspace's usefulness depends on both:

- the workflow template, which defines the data
- the views, which define how people interact with the data

### Shared Presentation Settings

View templates can change component visibility, labels, placeholders, and read-only presentation without changing the underlying field definitions. Supported settings depend on the surface: a collection can expose a compact set of fields, while a detail form can expose more. Forms also have [input settings](#form-overrides) such as requiredness and defaults.

## Collection Views

Collection views show multiple entities.

| View | Common Fit |
|---|---|
| List | Fast operational scanning, mobile use, compact status updates |
| Table | Dense data review, spreadsheet-like comparison, sorting, export-style work |
| Board | Visual process tracking grouped by status, stage, priority, or another groupable field |
| Calendar | Date-driven planning, due dates, schedules, inspections, appointments |
| Tree | Records organized through a configured parent relationship, useful for hierarchies |

### Choose A Collection

View choice follows the job:

- Boards fit work moving through a process.
- Lists fit quick operational queues.
- Tables fit dense comparison or reporting.
- Calendars fit date-driven planning.
- Trees fit records whose parent relationships form a useful hierarchy.
- Separate views can serve different roles or moments better than one overloaded view.

### Field Dependencies

Some collections depend on particular fields in the workflow template:

- Boards need a groupable field, usually a tag such as status, stage, or priority.
- Calendars need a selectable Date field for the scheduled or due moment. A managed Date endpoint derived from a Date Range can serve that role where the configuration offers it; the Date Range field itself is not the calendar's date selection.
- Trees need the relevant parent relationship among records.
- Useful dashboards need collections and fields that can be filtered by status, assignee, date, location, category, or other operational dimensions.

A central collection is easier to support when the template includes the fields it needs.

### Select And Organize Records

Saved collection views can narrow and organize records by fields such as status, assignee, priority, date, location, category, or current user. On the web, people can create and save filters, grouping, and sorting. Mobile renders those saved criteria and lets a person make temporary changes, but cannot save the changes. Those temporary changes do not alter the saved selection used by the team or a dashboard widget.

Common saved examples include My Open Work, Overdue Requests, Work Orders by Status, Preventive Maintenance Calendar, and External Requests Pending Review. Grouping and sorting then make the selected records easier to scan. A view's filter changes which records it presents, not which records a member is authorized to access.

### Collection Read-Only Behavior

A collection can prevent editing from that surface while still showing the underlying records. This is a presentation rule, not a complete access rule; see [People And Access](product-model.md#people-and-access).

## Card Views And Forms

### Card Views

A card view shows one card/workflow entity at a time. It can be used for:

- creating a new record
- viewing or editing a record
- presenting a simplified role-specific detail page
- collecting external form submissions
- sharing a read-only public detail page

Card views are where field order, sections, visibility, read-only behavior, and message-thread placement matter most.

### Form Overrides

In addition to shared presentation settings, a form's view template can set requiredness, defaults, prefilled values, and layout where supported. These settings govern the selected form without changing the underlying workflow template.

This matters for forms and role-specific views:

- External request forms can hide internal triage fields and set starting defaults.
- Internal card views can expose full operational detail.
- A simplified card view can show only the fields needed for a particular role.

Evaluate requiredness and layout in the selected form's configuration. A rule on one form does not establish the behavior of every way to create or update the record.

Subform create and update forms can likewise configure how embedded answers are entered and edited. Their view templates shape the form experience; the answers still belong to the parent card.

### Card Layouts

Card layouts organize fields inside a card view.

Strong layouts commonly:

- put the primary name/title and status first
- group related fields into sections
- keep short operational fields together
- put long notes and files where they have enough space
- hide or collapse finance, metadata, or automation-control fields unless they are important to the user

Layout reduces cognitive load. A complete workflow can still feel unusable if the card layout is just a long unordered field list.

### External Forms

External forms let non-Coast users submit cards/workflow entities through a public link.

Common external form pattern:

1. A public card view defines the form.
2. The form shows only fields the external submitter should fill out.
3. Internal triage fields, assignees, automation flags, and internal status details are hidden.
4. Defaults or hidden fields mark the submission source and starting status.
5. Automations route or notify internal users.

Example: an external work order request form might show title, description, files, requester name, requester phone, requester email, location, and asset. It might hide status, priority, assignee, internal notes, cost, and completion fields.

Distinguish the pieces:

- The external form is the public card view/form definition.
- The external form link is the shared distribution link, which can be used for sharing, QR codes, and prefilled-link patterns.

External forms are public surfaces. Inspect the selected form and its defaults from an external submitter's perspective, then use a representative test submission to confirm that internal user lists, assignee fields, statuses, and private operational data are exposed only when intended.

### Public Read-Only Views

A public read-only view or shared-card link shares record information without accepting new submissions. Common fits include status pages, shared request details, or public-facing record summaries where the recipient should see information but not edit or create data.

## Dashboards

Dashboards are widget-based surfaces for summaries, shortcuts, and saved attention across workflow data. An entity widget points at a workspace, workflow template, and collection view; it can count the records selected by that view and return someone to the underlying work. The widget does not own another copy of those records or a separate filter definition.

Common dashboard content includes:

- counts by status
- overdue work
- assigned work
- upcoming schedule
- inventory alerts
- request queues
- shortcuts to saved operational views

Dashboards are most useful when the underlying workspace has good status, date, ownership, relationship fields, and saved views. Weak data modeling leads to weak widgets and weak reporting.

### Widget Favorites

A person can favorite a dashboard widget; that server-stored association changes their widget shortcut list without changing the widget's collection or record selection. The dashboard's All/Favorites display choice is remembered locally by the client. This is distinct from a [workspace favorite or organization bookmark](product-model.md#workspace-navigation). Widgets and filtered collections can bring assigned, overdue, waiting, blocked, or upcoming work close to hand; the [activity feed](product-model.md#conversations-and-activity) serves the separate question of what changed recently.
