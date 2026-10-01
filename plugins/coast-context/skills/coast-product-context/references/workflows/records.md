# Records

A **record** is one instance of a [workflow template](templates.md). Coast also calls it a **card** or **workflow entity**. A Work Order template defines the fields and views; “Repair dock door 4” is a particular work order with its own values, identity, and discussion. Use the business noun when known, and “record” when explaining behavior shared across work orders, assets, locations, and other configured kinds.

## Identity and content

A standalone record belongs to a template and has a workspace context. Its values correspond to component definitions on that template. The template can select a Text component as the record's displayed title, so a changed title value changes what people see without replacing the record. The title is not guaranteed unique. Missing-title fallback text depends on the client; it is not a universal record-naming rule. Coast also gives records a stable UUID and can display a human-readable card number; a card number is useful for reference, while integrations and relationships must follow the identity contract of their operation. Neither a title nor a number should be inferred to be the template ID.

For example, a Work Order may contain an issue description, a configured Status tag, a due Date, and a Related Card selection pointing to an Asset. The Work Order and Asset remain distinct records even if they appear together in a card view. [Components](components.md#connected-records-and-derived-values) explains stored relationships, reverse displays, and live lookups. [Views](view-templates.md) determine which of the values a person sees and edits.

A Subform value is different from a Related Card selection. It keeps filled questions *inside* the parent record. Those answers can have their own structure and attachments, but they do not acquire another standalone record ID, workspace placement, or discussion thread. The subform definition is reusable; the filled value is parent-owned. [Subform](components.md#subform) gives the value and question model.

## Lifecycle and configured state

Creating a record supplies values for a selected template and workspace context. Updating changes values on an existing record. Duplicating creates another record rather than changing the source. The form and permissions used for an operation can determine what the person is asked to supply; [view-specific requiredness](view-templates.md#card-display-creation-and-update) is not a universal schema requirement on every write.

Records can be deleted from ordinary work views and found in a workspace trash experience by people with the relevant access; a deleted record can be restored there. Treat delete, trash, and restore as lifecycle operations, not as ordinary values in a customer's Status field. Conversely, “Open,” “Waiting,” and “Done” are examples of *configured* statuses, often Tag selections. Coast does not derive one universal approval path or status progression from the fact that something is a record. Automations and recurrence can create or change records only through their configured behavior; see [Automations](automations.md) and [Recurrence](recurrence.md).

Creation and update timestamps answer when those operations happened. They do not by themselves describe who changed each field, why a status moved, or the record's deletion policy. When the question is what changed, use the record's field-change or audit history where available, distinguishing those events from current values. An audit entry records an event; it is not another component value. Similarly, reordering cards within a collection view changes presentation order, not the workspace that owns the cards. [Record selection](record-selection.md#ordering-and-grouping) owns custom ordering.

## Discussion on a record

A standalone record has an attached conversation thread for comments and activity in context. A comment is a message associated with that record, rather than a Text component value or a new record. People can discuss a Work Order while its structured fields continue to represent its current operational data. [Conversations](../organization/conversations.md) explains the shared message and thread model, and [Search and navigation](../across-workspaces/search-and-navigation.md) explains how record comments can be found in search results.

## Modeling decisions

When deciding whether to create a record kind, ask whether each item needs independent identity, discovery, relationships, discussion, or lifecycle. A machine that many Work Orders refer to is a good candidate for an Asset record and a Related Card relationship. A reusable inspection procedure completed only as part of one Work Order is a good candidate for an embedded Subform. A separately scheduled inspection with its own discussion or status can be its own record. This choice affects where values and history live; naming a field “Asset” or “Inspection” does not make it an independent record.
