# Views, Forms, And Dashboards

This reference describes how people see, enter, scan, share, and summarize Coast workflow data.

## Contents

- [Views Are Experience Layers](#views-are-experience-layers)
- [Card Views](#card-views)
- [Collection Views](#collection-views)
- [View Fit](#view-fit)
- [Field Dependencies](#field-dependencies)
- [Filtering, Grouping, And Sorting](#filtering-grouping-and-sorting)
- [View-Level Overrides](#view-level-overrides)
- [Card Layouts](#card-layouts)
- [External Forms](#external-forms)
- [Public Read-Only Views](#public-read-only-views)
- [Dashboards](#dashboards)
- [Favorites And Saved Attention](#favorites-and-saved-attention)

## Views Are Experience Layers

View templates are the saved views people select. They control how cards/workflow entities appear without creating another view object or a copy of the data. The same record can appear in a list, table, board, calendar, card/detail view, or public shared-card surface. An external form collects a new record. Dashboards sit one level up: dashboard widgets summarize or link to filtered workflow views rather than acting as another record view.

This means a workspace's usefulness depends on both:

- the workflow template, which defines the data
- the views, which define how people interact with the data

## Card Views

A card view shows one card/workflow entity at a time. It can be used for:

- creating a new record
- viewing or editing a record
- presenting a simplified role-specific detail page
- collecting external form submissions
- sharing a read-only public detail page

Card views are where field order, sections, visibility, read-only behavior, and message-thread placement matter most.

## Collection Views

Collection views show multiple entities.

| View | Common Fit |
|---|---|
| List | Fast operational scanning, mobile use, compact status updates |
| Table | Dense data review, spreadsheet-like comparison, sorting, export-style work |
| Board | Visual process tracking grouped by status, stage, priority, or another groupable field |
| Calendar | Date-driven planning, due dates, schedules, inspections, appointments |
| Read-only collection behavior | Controlled display where users should inspect but not edit from the collection |

## View Fit

View choice usually follows the job:

- Boards fit work moving through a process.
- Lists fit quick operational queues.
- Tables fit dense comparison or reporting.
- Calendars fit date-driven planning.
- Separate views can serve different roles or moments better than one overloaded view.

## Field Dependencies

Some views depend on particular fields in the workflow template:

- Boards need a groupable field, usually a tag such as status, stage, or priority.
- Calendars need a date/date-range field that represents the scheduled or due moment.
- Useful dashboards need views and fields that can be filtered by status, assignee, date, location, category, or other operational dimensions.

A central view is easier to support when the template includes the fields that view needs.

## Filtering, Grouping, And Sorting

Views can narrow and organize records by fields such as status, assignee, priority, date, location, category, or current user.

Common examples:

- My Open Work
- Overdue Requests
- Work Orders by Status
- Preventive Maintenance Calendar
- External Requests Pending Review

The product idea is simple: views turn one shared workflow into focused work surfaces for different audiences.

## View-Level Overrides

Views can change the experience without changing the underlying workflow template. Depending on view type and current product/tooling support, a view can hide fields, mark fields read-only, mark fields required, set defaults, prefill values, change labels/placeholders, and control card layout.

This matters for forms and role-specific views:

- External request forms can hide internal triage fields and set starting defaults.
- Internal card views can expose full operational detail.
- List views can show only the fields needed for quick scanning.
- Read-only collection behavior can prevent editing from a collection while still showing the underlying data.

Requiredness, layout, and form behavior have differed across Low Code and No Code migration paths. Exact current builder/form behavior should be verified before promising required-field or layout behavior for migrated workflows.

## Card Layouts

Card layouts organize fields inside a card view.

Strong layouts commonly:

- put the primary name/title and status first
- group related fields into sections
- keep short operational fields together
- put long notes and files where they have enough space
- hide or collapse finance, metadata, or automation-control fields unless they are important to the user

Layout reduces cognitive load. A complete workflow can still feel unusable if the card layout is just a long unordered field list.

## External Forms

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

External forms are public surfaces, so internal user lists, assignee fields, internal statuses, or private operational data should stay out of them unless a current product decision explicitly supports that use case.

## Public Read-Only Views

A public read-only view or shared-card link shares record information without accepting new submissions. Common fits include status pages, shared request details, or public-facing record summaries where the recipient should see information but not edit or create data.

## Dashboards

Dashboards are widget-based surfaces for summaries, shortcuts, and saved attention across workflow data. Widgets commonly point at a workspace/template/view combination or summarize counts from filtered views.

Common dashboard content includes:

- counts by status
- overdue work
- assigned work
- upcoming schedule
- inventory alerts
- request queues
- favorites or saved operational views

Dashboards are most useful when the underlying workspace has good status, date, ownership, relationship fields, and saved views. Weak data modeling leads to weak widgets and weak reporting.

## Favorites And Saved Attention

People often need shortcuts back to the work that matters to them. Dashboards, filtered views, favorites, and role-specific views all serve the same product goal: reduce the distance between a user and the next useful action.

These surfaces often organize attention around:

- what is assigned to me
- what is overdue
- what changed recently
- what is waiting for review
- what is blocked or at risk
- what is coming up next
