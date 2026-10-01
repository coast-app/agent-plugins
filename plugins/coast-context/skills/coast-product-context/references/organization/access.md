# Access

Access answers whether a particular person may perform a particular action on an organization, workspace, record, or configuration. [People](people.md) owns account and membership identity; this chapter owns roles and grants. A visible control, a selected assignee, and an invitation are not authorization decisions.

## Organization roles

An organization membership carries an **Owner**, **Admin**, or **Member** role. The owner is the designated organization owner and can manage organization API keys. Owners and admins can manage organization-level billing and user groups. A member participates in the organization without those administrative grants merely by being a member. These are organization responsibilities, not a shortcut to every workspace record or builder action; establish the workspace grant and the action being attempted as well.

## Workspace access

An individual's **workspace membership** carries separate choices for record work and configuration.

### Record access

**Card Access** governs what the person can do with workflow records in that workspace:

| Level shown to members | Record work it permits |
| --- | --- |
| **View Only** (`VIEW`) | View records without creating, editing, or deleting them. |
| **Editor** (`EDIT`) | View and update records, but not create or delete them. |
| **Creator** (`CREATE`) | View, create, and update records, but not delete them. |
| **Admin** (`FULL`) | View, create, update, and delete records. This is a workspace card-access level, not an organization Admin role. |
| **Personal** (`PERSONAL`) | Create records; view, update, and delete existing workflow records that reference the person in a Person field. Other records in the workspace are outside that personal record scope. |

Comments follow their message and record context, so a card-access label alone is not a substitute for the applicable comment permission. Personal access does not create private copies of records, and creating a record does not by itself establish that it will match the person's later personal-view criterion.

### Configuration access

**Configuration** access answers which parts of the workspace's workflow a person can configure:

| Level shown to members | Configuration work it permits |
| --- | --- |
| **No Access** (`NO_ACCESS`) | No workspace configuration editing; card access may still permit record work. |
| **View Editor** (`VIEW_EDITOR`) | Create, edit, and remove views for the workspace's workflow, without changing its template or automations. |
| **Admin** (`FULL`) | Configure the workspace's workflow template, views, and automations, subject to the action's other checks. This is distinct from organization Admin. |

### Effective access

These describe the normal workflow-record and configuration roles, not a permission matrix for every object in Coast. The exact operation still needs an effective permission decision, including its target, plan, and any other applicable grant.

A group can also be associated with a workspace grant. Group association and individual membership are different sources of access, so the person's effective permission depends on the applicable grants and action, not one label read in isolation.

For example, a technician might be allowed to update a work order without being allowed to redesign the Work Orders template. A coordinator might manage view configuration for that workspace without gaining an organization-wide administrative role. When a person cannot see an assigned record, check workspace and record access; the [Person field](../workflows/components.md#person) that names them does not grant entry.

Changing a member's access is itself an authorized action. The ability to open a members screen or edit one row does not establish permission to change every person's access. Check the target member, scope, and requested change under the current authorization policy.

## Other limits on an experience

A view's hidden or read-only setting controls presentation or input in that experience; it is not a complete security rule. Public [forms and shared cards](../workflows/view-templates.md#public-forms-and-shared-cards) have their own link and access context. [Plans and billing](plans-and-billing.md) owns commercial entitlement and usage limits. These factors can each affect an experience without substituting for a person's authorization. A feature's presence in one client also does not establish its availability to every customer.
