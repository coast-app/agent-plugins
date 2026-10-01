# Workspaces and sections

A **workspace** is a place within an [organization](people.md) for a team to communicate and, when configured for structured work, manage records. The older API name is *channel*. A workspace has its own identity, name, membership, and conversation context. A workflow workspace also uses a [workflow template](../workflows/templates.md) and [views](../workflows/view-templates.md) for its records; a conversation context does not require every workspace to contain cards.

## Work and conversation

The workspace conversation supports general discussion about the team's work. A standalone record, such as one work order, has its own [discussion](conversations.md) attached to that record. These are distinct contexts even if the same people participate. A workspace's **default focus** selects a conversation-first or workflow-first entry experience. It changes what people encounter first, not the underlying meaning of a message or record. The web builder creates workflow configuration, while clients may emphasize different ways of using it.

Direct conversations are another communication context. They should not be described as an ordinary workflow workspace with empty records. [Conversations](conversations.md#conversation-contexts) explains the common message behavior; [Search and navigation](../across-workspaces/search-and-navigation.md) explains how people reach each context.

## Sections and placement

A **workspace section** is a named grouping of workspace entries in the organization. A Maintenance section might group Work Orders, Assets, and Parts. A workspace can belong to zero or one named section; adding it to a second section requires changing its placement, not making another simultaneous section association. Moving it between navigation groups does not move its records, discussion, organization, or [access grants](access.md). Section order and workspace placement help people find work; [search and favorites](../across-workspaces/search-and-navigation.md) are separate navigation choices.

The navigation may also show reserved groups such as Favorites or a general Workspaces group. Those groups are not necessarily persisted workspace sections. A section can be used as the source for sharing a collection of configured workspaces; the copy and installation semantics belong to [workflow distribution](../exchange/workflow-distribution.md).

## Lifecycle and configuration

Creating a workspace establishes a new work context and its initial configuration. Its name, description, membership, section placement, focus, workflow template, and views answer different questions and can evolve separately. Deleting or restoring a workspace affects the workspace's lifecycle; it is not a way to reclassify its records by changing a section. Confirm the supported operation and consequences for the affected organization before promising a restoration or retention outcome.

Installing a shared workflow also creates destination workspaces and active template copies. The installed configuration can be edited independently of its source. See [workflow distribution](../exchange/workflow-distribution.md) for source, destination, and copied identity rather than treating the installed workspace as a live link to the source.
