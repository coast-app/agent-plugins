# Coast Product Model

This reference gives the overall map of Coast: how the product is organized and how chat, workspaces, workflows, and cards/entities fit together.

## Contents

- [One-Sentence Model](#one-sentence-model)
- [Product Center](#product-center)
- [Hierarchy](#hierarchy)
- [Organizations And Businesses](#organizations-and-businesses)
- [Workspace Sections](#workspace-sections)
- [Users, Groups, And Membership](#users-groups-and-membership)
- [Workspaces](#workspaces)
- [Chat Threads](#chat-threads)
- [Workflow Templates](#workflow-templates)
- [Template Copies](#template-copies)
- [Workflow Bundles](#workflow-bundles)
- [Workflow Entities](#workflow-entities)
- [Views](#views)
- [Activity Feed](#activity-feed)
- [Product Mental Model](#product-mental-model)

## One-Sentence Model

Coast is a no-code platform where maintenance-heavy and operational teams communicate and manage structured work in configurable workspaces.

## Product Center

Coast is broadly configurable, but its product center is deskless operations and CMMS-style work: maintenance teams, work orders, preventive maintenance, assets, locations, parts, vendors, requests, inspections, and operational reporting.

That context helps interpret examples. Coast can model many workflows, but the clearest examples usually come from maintenance, field service, facilities, operations, and other teams that need structured work plus team communication.

## Hierarchy

```text
Organization / business
  -> Workspace sections
    -> Workspaces
      -> Chat thread
      -> Workflow template
        -> Cards / workflow entities
          -> Card/entity discussion thread
```

## Organizations And Businesses

An organization is the top-level customer/account boundary in Coast. Internal APIs and MCP resources often call this a business. Access, users, workspaces, and data are scoped to an organization/business. A person can belong to multiple organizations/businesses, but a given workflow or workspace belongs to one context.

In general explanations, organization is usually the clearest term unless the task is discussing API, MCP, or implementation details that use business.

## Workspace Sections

Workspace sections organize workspaces into navigation groups. They are not the workflow itself; they help people find related spaces.

Examples:

- Operations
- Maintenance
- Sales
- Support
- HR
- Locations

## Users, Groups, And Membership

Users are people in an organization/business. Workspaces have members, and membership plus role settings help determine what a person can see or do in that workspace.

User groups can help manage sets of people. They are useful for assignment or permission management when a workflow needs to reason about teams rather than one person at a time.

Common access ideas:

- read-only viewing
- editing or contributing
- full administrative control
- private/personal access for direct messages

Role labels vary by product surface, but common vocabulary includes organization-level owner/admin/non-admin ideas and workspace-level admin/editor/view-only or personal access. Exact labels can be support- or implementation-sensitive.

## Workspaces

Workspaces are the central container in Coast. A workspace has:

- a chat thread for messages and collaboration
- members and access levels
- cards/workflow entities, if the workspace is backed by a workflow template
- views that show workflow data in different ways

The important product idea is that workspaces combine communication and structured work. A team can discuss work in the same place where the work is tracked.

## Chat Threads

Coast has workspace-level chat and card/entity-level discussion.

- Workspace chat is for general discussion in the workspace.
- Card/entity threads are attached to individual records, so discussion stays connected to the exact work item.

Example: a Work Orders workspace can have a general team thread, while each work order has its own comment thread for diagnosis, updates, files, and decisions.

## Workflow Templates

A workflow template defines the shape of a workflow. It is similar to a schema, form definition, or database table design, but it is composed from Coast components instead of code.

A template answers:

- What kind of records exist here?
- What fields do records have?
- Which relationships connect records to other workspaces?
- Which fields are editable, required, hidden, read-only, sortable, or filterable?
- Which views and automations make the workflow useful?

## Template Copies

Workflow library and bundle templates are starting points. When a workflow workspace is created or a bundle is installed, Coast creates active template copies for the resulting workspace(s).

This distinction matters:

- A source/library template defines what can be installed.
- A workspace's copied workflow template is what live cards, views, automations, and later edits use.
- Source and installed template IDs identify different templates.

## Workflow Bundles

Workflow bundles are predefined collections of workspaces, templates, views, dashboards, and automations for common business scenarios.

Bundles are starting points, not locked products. After a bundle is installed, the created workspaces and templates can be customized to match the customer's process.

Coast curates the library's bundle listings. Customers can share a workflow workspace or a section of workspaces directly through an install link without publishing it to the library or entering an approval process. Either path installs customizable copies of the source configuration.

## Workflow Entities

A workflow entity is a single record made from a workflow template. In current UI/customer language, this is usually a card or the specific business noun.

Examples:

- work order
- asset
- location
- procedure
- inspection
- cost entry
- support request
- customer account
- sales lead

Each card/entity has structured field values. Standalone cards, such as work orders and assets, have their own discussion threads.

## Views

View templates are saved configurations for presenting existing cards/workflow entities or collecting input for new ones. They do not create separate view objects or copies of the data.

The same cards/workflow entities can appear in multiple views:

- all records in a table
- active work grouped by status on a board
- due work on a calendar
- one card/entity in a card/detail view

A public external request form collects a new card/entity. A public shared-card view shows an existing record read-only.

This separation matters: change the view template to change the experience; change the workflow template to change the underlying data model.

## Activity Feed

The activity feed shows recent activity across Coast. It is useful for understanding what changed, who acted, and where attention may be needed. It complements workspace chat and entity threads by giving a broader chronological view of activity.

## Product Mental Model

A compact mental model for Coast:

- Workspaces are places where work happens.
- Templates define the shape of the work.
- Cards/entities are the actual work records.
- Components are the fields and interactive pieces.
- Views make the same work usable for different people and moments.
- Automations make explicit things happen when records are created, updated, or manually acted on.
- Bundles package common starting points, but teams can customize the resulting workspaces.
