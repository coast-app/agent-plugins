# Workflow Building Blocks

This reference describes the reusable pieces that make up Coast workflows, fields, records, relationships, and subforms.

## Contents

- [Workflow Templates](#workflow-templates)
- [Workflow Entities](#workflow-entities)
- [Components And Fields](#components-and-fields)
- [Tags](#tags)
- [Person Fields](#person-fields)
- [Relationships](#relationships)
- [Lookups](#lookups)
- [Subforms](#subforms)
- [Relationship Quantities](#relationship-quantities)
- [System Fields](#system-fields)
- [Field Archiving](#field-archiving)
- [View-Aware Field Context](#view-aware-field-context)
- [Useful Modeling Questions](#useful-modeling-questions)

## Workflow Templates

A workflow template defines the structure for a class of records. If the workspace is "Work Orders," the workflow template defines what a work order contains: title, status, requester, assignee, due date, related asset, notes, files, and so on.

Templates usually map to durable business concepts, not just UI screens. A strong template supports the lifecycle of the real-world thing it represents.

## Workflow Entities

Workflow entities are the records created from a template. They are the live units of work.

Examples:

- a specific repair request in a Work Orders workspace
- a forklift in an Asset Management workspace
- a store in a Locations workspace
- a single cost line item linked to a repair
- a procedure/checklist attached to an inspection

The specific business noun is usually clearer when one exists. "Work order" or "asset" reads better than "workflow entity" in most human-facing contexts.

## Components And Fields

Components define the fields and interactive elements on cards/workflow entities.

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

## Tags

Tags are best for finite states, categories, and process stages.

Good tag examples:

- Status: Open, In Progress, Done
- Priority: Low, Medium, High
- Approval stage: Draft, Review, Approved
- Source: Internal, External

Lists of real-world things that grow or have their own details are usually not tag-shaped. Stores, locations, assets, customers, vendors, and employees usually deserve their own workspace and relationship fields.

## Person Fields

Person fields refer to Coast users. They commonly support assignments, ownership, approvals, watchers, or internal recipients.

A person field is different from a domain relationship. For example, a Technician field can assign work to a Coast user, but a Vendor, Customer, Location, or Asset usually belongs in its own workspace and is linked as a relationship.

## Relationships

Relationships connect cards/workflow entities.

The core relationship field is one-way:

- Related card: the active link from one entity to another.

Coast often pairs that with a reverse display:

- Referenced in: a target-side display that shows which other entities link to this one.

Example:

- A Work Order has a related Asset.
- The Asset can show referenced Work Orders.

Relationships make Coast useful for systems of work rather than isolated forms. In high-cardinality relationships, the active related-card field usually sits on the side where users select the relationship, with referenced-in providing reverse visibility instead of a second manually maintained relationship.

## Lookups

Lookups display information from a related entity. They help users avoid duplicating data manually.

Example: a Work Order can relate to an Asset, and then show the Asset's Location or Serial Number as context.

Lookups fit situations where people need to see related information without manually editing duplicate copies.

## Subforms

Subforms are embedded structured data inside a parent entity. They are useful for checklists, inspections, procedures, or repeatable sections that should live inside one record rather than as a full separate workspace.

Subforms are backed by subform workflow templates. They can have their own field shape and form behavior, but they should still be treated as embedded structure inside the parent card, not as independent operational workspaces.

Subforms fit situations where:

- the data belongs inside one parent record
- it does not need independent collection views, dashboards, reporting, ownership, or cross-workspace relationships
- the user experience benefits from an embedded form/checklist

Separate workspaces fit situations where:

- child records need independent ownership, status, reporting, or lifecycle
- users need to scan, filter, assign, or report on child records independently
- other records need to link to the child records directly

Current subform work has had component allowlist and nested-subform constraints. Current builder support should be verified before promising a specific component type inside a subform.

## Relationship Quantities

Some relationships carry a quantity. For example, a Work Order might link to a Part and specify that it used 3 units.

This is different from a part's own inventory count. The relationship quantity describes the link; the part's inventory count describes the part itself.

## System Fields

Coast records also have system-managed metadata such as creator, creation time, update time, sequence number, and links. These are useful for display, sorting, filtering, and auditing, but they are not normal user-authored fields.

## Field Archiving

Archived fields are different from hard-deleted fields. Archiving can preserve the semantic meaning of existing card data and make unarchive/restore paths possible. Hard deletion can break existing cards or leave data without enough structure to recover meaning.

Existing workflow changes usually preserve data and field meaning unless a current product/tooling guide explicitly says removal is safe.

## View-Aware Field Context

Field types shape how people can use the data:

- Tag fields support board columns, status pipelines, categories, and filterable states.
- Date or date range fields support calendars, scheduling, and time-relative reminders.
- Number fields support metrics, quantities, thresholds, and intervals that need sorting, arithmetic, or rollups.
- Related-card fields support real-world things that need their own records, details, and reporting.

## Useful Modeling Questions

Questions that help interpret workflow-shaped product context:

1. What things do users track?
2. Which things need their own records?
3. Which things are just fields on another record?
4. Which relationships matter?
5. Which people need which views?
6. Which changes should trigger automations?
