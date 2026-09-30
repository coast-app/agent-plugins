# Search and navigation

Navigation helps a person return to known places; search helps locate a person, workspace, record, or message from a clue. Both cross workspace boundaries, but neither changes the underlying records or their [access](../organization/access.md). A named view or dashboard is a saved way to focus work, not a universal search result.

## Returning to a destination

People move among their organizations, workspaces, direct conversations, dashboards, reports, and support or help entry points through client navigation. Named [workspace sections](../organization/workspaces.md#sections-and-placement) group workspace entries. Changing a navigation group does not change a workspace's data or permissions.

A **workspace favorite** is a client-local personal shortcut to a workspace, not a server-backed workspace section or membership grant. Its visibility on another device should therefore not be assumed. A **bookmark** is a named URL saved in an organization; it points to a destination rather than selecting records. A workspace favorite is different from a [favorite dashboard widget](dashboards.md#personalizing-a-dashboard), whose association is server-backed.

## Search scopes

Search controls answer different questions:

| Search | What it looks for | Where a result leads |
| --- | --- | --- |
| Workspace and people discovery | Names in the relevant navigation or directory context. | The workspace or person. |
| Global card and message search | Matching cards and messages from accessible work contexts, including messages in record discussions. | The record when a message belongs to one, or the workspace conversation otherwise. |
| Collection search | Records within a particular workflow collection. | A matching record within that workspace and view. |
| Related Card picker search | Eligible records for one configured relationship. | A selection for the field being edited. |

One search UI can combine several result groups without using one matching rule or index for all of them. A card's comment appears as a **message with record context**, not as a separate type of card result. Match scope and destination to the person's question before treating a missing result as absent data. Search ranking and completeness depend on the current search surface and accessible population.

Collection views also have saved and temporary filters, groups, and sorting. [Record selection](../workflows/record-selection.md) owns the criteria meanings, while [View templates](../workflows/view-templates.md#saved-criteria-and-temporary-adjustments) owns saved configuration versus a person's current browsing changes. Search terms and view criteria should not be described as the same saved object.
