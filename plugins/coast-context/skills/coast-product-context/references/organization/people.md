# People and organizations

An **organization** is the customer context for people, workspaces, configuration, and data. APIs and older Coast language may call it a *business*. A person can work in more than one organization; selecting an organization determines which membership and workspaces apply to the work at hand. [Workspaces](workspaces.md) describes where that work happens, and [Access](access.md) describes what a member may do.

## Account and membership

A **user account** is one person's login identity and profile, including their name and contact details. **Organization membership** associates that account with an organization and its role. The profile is not a separate person for each organization, while roles and workspace access are specific to their context. If someone belongs to Facilities and also to a contractor's organization, their account can be the same in both, but their Facilities role does not carry into the contractor's work.

The organization directory concerns people in that organization. A [Person field](../workflows/components.md#person) on a record selects a Coast user for a customer-defined purpose such as assignment. That field stores record data; assigning a work order does not add the assignee to the organization or grant access to its workspace. Conversely, membership does not assign the person to a particular record.

## Joining and leaving

An **invitation** offers a path to join or activate membership. A shareable invite link, a direct invitation, a pending person in the directory, and an active organization member are different states or surfaces. An invitation alone is not evidence that the recipient has access. Check the resulting membership and applicable [permission](access.md) before treating a person as able to see or change work.

Membership may later be changed, deactivated, or removed. These operations affect the person's relationship to that organization; they are not the same as editing their account profile or removing a [workspace membership](access.md#workspace-access). A pending marker should not be used as a universal authorization rule: the relevant action and current membership determine access.

## User groups

A **user group** is a named set of people within an organization. It can help manage teams and can be associated with workspace access, but group membership and a workspace grant are separate relationships. Do not infer a person's effective permission from the group's name or from assignment to a Person field. Use [Access](access.md#workspace-access) to evaluate the grant for the workspace and action. Group management may depend on the organization's available configuration.
