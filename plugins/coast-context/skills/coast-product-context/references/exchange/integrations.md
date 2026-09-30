# Provider integrations

An **integration** is an organization's configured connection to an external provider. The provider's app descriptor identifies an available connection type; an integration instance belongs to a customer organization and can also be associated with a workspace. The descriptor is neither the customer's completed connection nor a Coast client app or an installable [workflow bundle](workflow-distribution.md).

## Connection and behavior

An integration instance has its own setup state and provider configuration. Completing setup matters: a provider shown in a catalog does not mean an organization has connected it, and a connection being configured does not establish that it is active. People with the applicable [access](../organization/access.md) can manage a connection according to the provider's supported setup flow.

The provider contract and customer configuration determine what data moves, its direction, timing, field mapping, and failure behavior. There is no universal rule that every connection continuously synchronizes records or resolves conflicts. Some configured connections can send events about record changes to a provider; event delivery is distinct from importing provider data back into Coast. A webhook destination similarly does not prove bidirectional reconciliation or a delivery guarantee.

When a request says “connect Coast to” another system, identify the provider, organization, affected workspaces or record kinds, and desired exchange. A one-time [import](import.md) or [export](export.md) may satisfy a transfer request without establishing a provider connection. Conversely, a required ongoing exchange needs the specific provider's supported operations and an active customer connection.

API and MCP tools are programmatic routes to product operations, not integration instances merely because another application calls them. Their current access and operation contracts belong to their tool documentation. The [product model](../product-model.md) explains how those routes relate to Coast objects.
