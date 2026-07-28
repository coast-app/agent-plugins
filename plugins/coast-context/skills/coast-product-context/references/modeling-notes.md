# Coast Product Modeling Notes

This reference explains how Coast product primitives tend to map to real operational work. It is useful context for building, using, selling, marketing, testing, managing, or explaining Coast, not a required recipe for a specific workflow.

## Contents

- [Composability Over One-Off Features](#composability-over-one-off-features)
- [Real-World Things As Entities](#real-world-things-as-entities)
- [Tags As States And Categories](#tags-as-states-and-categories)
- [Tracking Vs. The Tracked Thing](#tracking-vs-the-tracked-thing)
- [Snapshot Vs. Live Reference](#snapshot-vs-live-reference)
- [Shared Lifecycle, Shared Template](#shared-lifecycle-shared-template)
- [Fields Shape Views](#fields-shape-views)
- [Views Reflect Jobs](#views-reflect-jobs)
- [Internal Control Fields](#internal-control-fields)
- [Existing Data Meaning](#existing-data-meaning)
- [Explicit Behavior](#explicit-behavior)

## Composability Over One-Off Features

Coast is built from reusable primitives:

- workspaces
- workflow templates
- cards/workflow entities
- components
- views
- relationships
- automations
- dashboards

Coast is usually clearest when described as combinations of these primitives instead of a separate product concept for every business scenario.

## Real-World Things As Entities

If something has its own lifecycle, attributes, owner, views, reporting, or relationships, it is often entity-shaped in Coast: a workspace and cards/workflow entities.

Common entity-shaped examples:

- locations
- assets
- vendors
- customers
- employees or contractors when they are operational records
- parts
- procedures
- requests
- work orders

Growing lists of real-world things are usually better represented as records than as tag options. Tags fit statuses and finite categories better than locations, assets, vendors, customers, or other things with their own details.

## Tags As States And Categories

Tags fit finite sets where the tag itself does not need more data.

Common tag-shaped examples:

- status
- priority
- stage
- source
- category
- risk level
- approval state

Questions like "who owns this thing," "where is this thing," "what are its details," or "show me all work connected to this thing" often indicate that the concept is entity-shaped instead of tag-shaped.

## Tracking Vs. The Tracked Thing

Repeated child activity with its own lifecycle is often distinct from the record being tracked.

Example:

- A Work Order is the tracked thing.
- Time entries, cost entries, downtime events, and part usage can be separate tracking records related to the Work Order.

This supports multiple entries, independent edits, reporting, and auditability.

## Snapshot Vs. Live Reference

Sometimes a field intentionally duplicates information from a related entity because it captures a point-in-time snapshot.

Example:

- An Asset has a current operational status.
- A Work Order might also store the asset's operational status at the time the work was reported or performed.

The asset field is the live state. The work order field is historical context.

## Shared Lifecycle, Shared Template

Labels like "request," "task," "preventive maintenance," or "issue" do not automatically require separate templates when the records share most fields and lifecycle.

One template plus a category or type field tends to fit when:

- records can change classification
- most fields are shared
- views can filter by category
- automations can branch by category

Separate templates tend to fit when:

- records have different lifecycles
- different teams own them
- different views and reporting are needed
- the fields barely overlap

## Fields Shape Views

Field types determine which views, dashboards, automations, and reports can work well later.

- Boards need tag-like groupable fields.
- Calendars need date or date-range fields.
- Dashboards need fields that support meaningful filters and counts.
- Automations and rollups need numbers for quantities, metrics, thresholds, and intervals.
- Relationships need related-card fields, not text labels, when people need navigation, reporting, or reuse.

The same card form can look fine while still limiting later views, dashboards, automations, and reporting.

## Views Reflect Jobs

An "All Items" view is complete, but it is rarely the only useful surface.

Common focused view concepts include:

- triage
- my work
- overdue work
- status boards
- schedules
- reports
- external submissions
- read-only sharing

Views often represent the moment, role, or question someone has when they return to the work.

## Internal Control Fields

Some fields exist for automation, filtering, defaults, bridge relationships, or internal bookkeeping. They are often hidden, read-only, or placed in a low-priority section.

Examples:

- source fields
- hidden relationship bridges
- computed totals
- internal flags
- audit metadata

Users typically need the fields that help them make decisions, not every implementation detail.

## Existing Data Meaning

Existing workflow fields can carry live customer data. Archiving a field is often safer than hard deletion because it preserves enough structure to understand and potentially restore historical card data.

Removing fields, relationships, view templates, or automation-control fields can affect existing cards, dashboards, forms, integrations, and training material.

## Explicit Behavior

If a workflow depends on a value changing, a notification sending, a child record being created, or a related record updating, that behavior generally exists as explicit configuration.

Explicit behavior makes workflows more predictable, auditable, and easier to debug.
