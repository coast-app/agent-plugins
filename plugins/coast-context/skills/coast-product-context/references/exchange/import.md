# Importing records

An **import** brings external data into an existing Coast record kind. Start with the destination [workflow template](../workflows/templates.md): it defines the available fields and the kind of records the operation can write. Importing rows into that kind differs from installing a [workflow bundle](workflow-distribution.md), which copies configuration, and from a [public form](../workflows/view-templates.md#public-forms-and-shared-cards), which collects an individual submission.

## Target and preparation

The import entry flow is scoped to an organization, an authorized person, and a workflow template that accepts bulk import. Creating an import session opens a separate data-preparation experience. Source columns must be interpreted against the target fields, including component type, required form rules where applicable, and relationships to existing records. Opening the session does not by itself create records or establish a field mapping.

The session can carry a choice to suppress automations for its writes. This matters when an [automation](../workflows/automations.md) responds to record creation or update: the import and the rule together determine the resulting effects. A workflow that depends on a notification, related-record update, or inventory calculation should establish whether the chosen import path emits that trigger before promising the effect.

## Resulting records and outcomes

The import setup and result determine how source rows become records. Before submitting a file twice or using import to change existing records, check the importer’s mapping and matching options for that job. A successful session opening does not establish an update, merge, or deduplication policy. The resulting records have Coast identities and belong to the selected workflow; the source file remains an external input, not a continuing synchronization connection.

Review row errors and the resulting records before treating an import as complete. A session opening successfully is only the start of the operation. If the requested behavior depends on deduplication, overwrite rules, relationship resolution, or partial failures, establish those from the active import operation and its result rather than assuming a universal policy. [Integrations](integrations.md) covers configured provider connections; [Records](../workflows/records.md) owns the records after import.
