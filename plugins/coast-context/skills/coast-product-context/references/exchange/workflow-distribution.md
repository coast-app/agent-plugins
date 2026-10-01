# Workflow distribution

Workflow distribution makes an existing configuration available as a starting point elsewhere. A **bundle** packages a workflow workspace or a workspace section with its workspaces and can include source card data. A library listing, customer installation link, source bundle, and installed workspace are distinct objects. Installation creates independently configurable copies rather than subscribing the destination to source changes.

## Curated library and customer sharing

Coast manually curates the workflow library. A **listing** supplies discovery information such as a name, description, and tags, and points to an enabled bundle. Browsing or previewing a listing shows the source configuration available for installation; the listing is not an installed workspace. Several client entry points can show the same library.

Customers can separately enable sharing for a workflow workspace or a section, choose whether to include card data, and distribute an installation link. This direct share does not create a library listing or submit a workflow for library approval. For example, a business can share one location’s maintenance setup with another location. A section source can distribute several related workspaces together. The source's configured components, views, and automations provide the starting behavior for those workspaces.

## Installation and copied identity

Installing a workspace bundle creates a workspace in the destination organization. Installing a section bundle creates a new section and its associated workspaces. The installation flow can collect a destination name, members and access choices, and a card-data choice. The source's choice to include card data and the installer's choice to include it both matter; an install link is not a live card link.

The installed workspace and active workflow template have new identities. Template lineage can record where the copy came from, but the copy is edited independently. Views, layouts, automations, and references to copied workspaces or templates are mapped into the destination configuration. Component IDs can remain the same within a copied template, so interpret a component ID together with its owning template. A preserved component ID does not make the source and installed field one shared object. Reinstalling produces another copy, not an update to an earlier installation.

The installation result identifies the destination workspace or section and maps source workflow-template IDs to their installed copies. That mapping covers templates; it is not a universal map for component or view IDs. Read the installed template and its views before using field or presentation identities.

Some configuration must be revalidated for the destination. An automation that is invalid after copying can be disabled, and a reference that cannot resolve may be omitted. A preview describes the source starting point; inspect the installed configuration before relying on its operational behavior. Source lineage is provenance, not synchronization.

An installation link distributes copyable configuration. A [public form or shared-card link](../workflows/view-templates.md#public-forms-and-shared-cards) exposes a live creation or reading experience; an [export](export.md) produces an external artifact. These serve different requests even when all are described as “sharing.” [Templates](../workflows/templates.md) owns the active record definition, and [Workspaces](../organization/workspaces.md) owns the destination container.
