# Maintenance workflow example

This example connects work orders, preventive maintenance, assets, downtime, parts, costs, and requester intake. It illustrates one customer's possible field contracts and operational choices; inspect the actual customer process and configuration before adapting it. For reusable MCP construction, follow the linked recipes. For product meaning and record-boundary decisions, use `coast-context:coast-product-context`.

This example follows the work from [record design](#records-and-fields) through [views and forms](#views-and-forms), [card layouts](#card-layouts), [list rows](#list-rows), [automations](#automations), and a [dashboard](#dashboard). Each section links to the reusable construction recipe.

## Records and fields

### Work Orders & PMs

#### Dual Identity: Work Order vs. Preventive Maintenance

In this example, Work Orders and preventive-maintenance records share a template because they share most fields. A **Category** Tag carries the chosen classification and can drive different automations.

Use one template when a record can change classification while retaining its identity and field values. Separate templates can be clearer when the record kinds have different identities, permissions, or lifecycle contracts. The customer's process decides.

#### Requester Fields Alongside Assignee

| Field | Purpose |
|---|---|
| Requester Name | The person who REPORTED the problem — may not be a Coast user |
| Requester Phone | Contact for follow-up questions about the problem |
| Requester Email | For automated status notifications back to the requester |
| Assignee | The technician ASSIGNED to do the work — always a Coast user |

An external form can collect Requester fields; configure their defaults and prefill explicitly in the selected view or link. Assignee is a Person field selecting a Coast user. These serve different roles: the requester reports the problem, while the assignee is assigned work.

#### Estimated Duration AND Estimated Cost

Both are planning fields set BEFORE work begins:
- **Estimated Duration** — Used for scheduling and workload balancing
- **Estimated Cost** — Used for budget approval before execution

This example tracks actual cost and time as [related Time Tracking](#time-tracking) and [Costs](#costs) records because each work order may have several entries with their own lifecycle. Other processes can record time in a Timer component on the work order or use a different cost structure. Compare estimates and actuals only when the workflow captures both consistently.

#### Asset Operational Status (on the WO, not just the Asset)

The WO template has its own **Asset Operational Status** Tag with three options: Operational, Partially Operational, and Non-Operational. It records the condition associated with this work order and can change during execution; it is not a live Lookup from the Asset workspace.

A technician can update this field during repair, and a configured automation can copy the chosen condition to the Asset's current-status field. After work ends, the work order retains its last recorded condition while the Asset can continue to change. If the process must preserve the condition *at the time of the initial report* as well, use a separate recorded value; edits to this one Tag cannot preserve all earlier observations.

#### Created from Checklist

The TAG `Created from Checklist` (Yes/No, default No) tracks WOs created by an optional Inspection extension. An automation on the Inspection template can react when its status changes to "Complete - Requires Attention," create a WO, set its **Inspection** RELATED_CARD back to the source record, and set this flag. Configure that status, relationship, and write before using the filter. This lets managers distinguish inspection-generated WOs from manually created ones.

#### Source (Internal vs External)

The **Source** Tag is read-only in ordinary views and defaults to the option labeled "Internal Request." This example gives external submissions the option labeled "External Form," whose Tag value is `"external"` (`["external"]` in field writes and conditions). This enables:
- Filtering views to show only external requests (for triage)
- Conditional email notifications (external requesters get email updates)
- Reporting on request origins

#### Recurring work in operational queues

This example uses the [recurring queue recipe](../configure-recurring-work.md#optional-reveal-near-the-due-date): Due Date anchors the series, Category defaults classify its occurrences as PMs, and a hidden Visible Tag distinguishes work ready for an operational queue from upcoming work. Ordinary records and the first occurrence start visible; upcoming occurrences start hidden.

A hidden **Queue Visibility** Scheduled Automation component tied to Due Date invokes **Make Recurring Card Visible** one day before the occurrence is due. That enabled `ADHOC` rule updates the Visible Tag. Configure and verify the series, defaults, and reveal rule before creating the filtered queues below. Calendar and PM planning views can include upcoming work. The separate **Reminders** component schedules notifications rather than revealing records.

### Time Tracking

Each labor entry has a **Labor Timer** and a **Work Order** RELATED_CARD selecting its parent. The work order's **Time Tracking** REFERENCED_IN displays all entries through that link. The work order also has a hidden **Current Labor Entry** RELATED_CARD selecting one entry for its status automation to stop; this stored selection is not the history of labor entries. The [bridge automation](#bridge-automations-entity_created) sets it when an entry is created.

### Asset Management

#### Self-Referencing Hierarchy

**Parent Asset** (`asset`) is a RELATED_CARD pointing to the SAME Asset Management template. This creates a tree: Building → Floor → HVAC Unit → Compressor. A configured view or calculation can use the hierarchy to roll up maintenance history; the link alone does not write a parent total.

#### Depreciation Field Group

| Field | Type | Purpose |
|---|---|---|
| Asset Value | NUMBER (CURRENCY) | Original purchase price |
| Salvage Value | NUMBER (CURRENCY) | Expected value at end of life |
| Depreciation Rate | NUMBER (PERCENT) | Annual depreciation percentage |
| Depreciation Start Date | DATE | When depreciation begins |
| Depreciation End Date | DATE | When asset is fully depreciated |
| Useful Asset Life | NUMBER (FLOAT) | Years of expected service |
| Depreciation Type | TAG | Straight Line vs. other methods |
| Depreciation Rate Category | TAG | Grouping for accounting |
| Disposal Method | TAG | How the asset will be decommissioned |
| Disposal Date | DATE | When disposed |
| Last Depreciation Review Date | DATE | When someone last reviewed the calculation |

These are accounting/finance fields that most maintenance technicians never touch. They exist because the asset register serves both operations AND finance. In a card layout, these should be in a collapsible section.

#### Meter Fields

| Field | Purpose |
|---|---|
| Current Meter Reading (readonly) | Computed from Meter workspace entries |
| Send Meter Alert Every... | Threshold for meter-based PM triggers |
| Meter Value - Create WO every (readonly) | Running count for meter-based WO creation |

Meter-based maintenance is an optional extension: readings in the Meters workspace select their Asset through a RELATED_CARD, and a configured calculation or automation owns the Asset's displayed current reading and count. A separate threshold-crossing rule can create a WO based on usage (e.g., every 500 hours) and set its **Meters** RELATED_CARD to the readings that prompted the work. That optional work-order field appears in the detail layout below. The threshold is set on the Asset; linking a reading alone neither updates those values nor creates a WO.

#### Checkout Tracking

**Checked out by** (PERSON) and **Checked-in Status** (TAG: Checked Out / Checked In) track portable assets like tools, vehicles, or test equipment. Simple binary state — who has it and is it available.

### Parts Inventory

#### Quantity System

Parts Inventory uses `hasQuantity: true` on the RELATED_CARD from Work Orders. This means when a WO links to a Part, it specifies HOW MANY of that part it needs. The Parts template then uses REFERENCED_IN_QUANTITY_SUM to compute remaining inventory.

In this design, **Initial Quantity** is set when stock is received. **Parts Quantity** is calculated by an automation from that value and recorded consumption. Protect the calculated field from ordinary manual edits, and decide how receipts, returns, and edits to prior usage affect it.

### Locations

This example models Locations as records with an Address component and relates other records to them. A separate Location record is useful when the place needs its own attributes or relationships. Check actual view and client support before promising a map experience.

### Downtime Tracking

Each Downtime record has a **Timer** and a **Service Request** RELATED_CARD selecting its work order. The work order's **Downtime** REFERENCED_IN displays all linked intervals. Its hidden **Current Downtime** RELATED_CARD selects one interval for the stop automation, and the [bridge automation](#bridge-automations-entity_created) writes that selection when a Downtime record is created.

#### Timer Auto-Start

The example Downtime Timer has `autoStart: true` for its editable client form. The status automation explicitly supplies the Timer's `START` dynamic value when it creates the Downtime record; `autoStart` is not a server-side creation rule. Verify the resulting interval, especially if records can be imported or backdated. A timer does not by itself define when an asset is operational again.

#### Downtime Type

The TAG "Downtime Type" (Planned / Unplanned / Predictive) drives reporting:
- **Planned** = scheduled maintenance downtime (expected, budgeted)
- **Unplanned** = breakdown/failure (unexpected, costly)
- **Predictive** = flagged by condition monitoring before failure

The ratio of unplanned-to-planned downtime is a key maintenance KPI.

### Costs

The Costs workspace is a ledger that links to Work Orders via RELATED_CARD. Each Cost entity represents a single line item — labor, material, contractor, etc. Separating costs from WOs allows:
- Multiple cost entries per WO (a single repair might have labor + parts + contractor costs)
- Different cost types with their own fields
- Financial reporting independent of work order status

## Modeling decisions illustrated

1. **Separate independently managed entries** — Use related records when time, cost, or downtime entries need their own identity, discussion, discovery, or lifecycle. A Timer or Number field can be simpler when the value belongs directly to one work order.

2. **Recorded condition versus live reference** — The Work Order Tag retains its last recorded condition after the work; a Lookup answers what the Asset says now. If the condition at reporting time must be immutable, capture it in a separate field or event.

3. **Define the write owner** — For a calculated field, identify the event and action that updates it, then present it as read-only where people should not edit it. View read-only settings do not replace write authorization.

4. **Keep internal controls out of ordinary views** — Hide automation-maintained fields where they would distract or invite accidental edits. Preserve an appropriate administrator or diagnostic path when people need to inspect them.

5. **Share templates when identities and field contracts align** — A Category Tag can branch behavior for related cases; separate templates remain valid when records differ materially.

6. **Choose the appropriate identity** — This example links Assets, Locations, Vendors, and Parts as domain records. Assignee selects a Coast user, while Created By displays system attribution. Other workflows can use Person fields for other user-related purposes.

## Views and forms

Use [choose collection and card views](../choose-record-views.md) to select each saved view and its filters. The following view inventory belongs to this one maintenance configuration.

### Work Order view set

These saved collections own their criteria. In this example, dispatchers triage Pending Review requests before technicians work them; operational queues exclude Pending Review and Complete. “Overdue” means Due Date is before today, rather than work whose Start Date has passed. Use the actual component and option IDs returned by the template.

| Saved collection | Selection and organization |
| --- | --- |
| Status List, Priority List, Location List | Visible and neither Pending Review nor Complete; grouped by the named field. |
| Assigned to me | The same operational selection, plus the current user in Assignee. |
| Overdue Work Orders | The same operational selection, plus Due Date before today. |
| External Requests Pending Review | Source value `external` and Status Pending Review; the dispatcher's intake queue. |
| PM List View | Category PM, including upcoming work for planning. |
| Board View | All records, grouped by Status. |
| Table View | All records, unfiltered. |
| Calendar View | All records, positioned by Due Date. |
| PM Calendar View | Category PM, positioned by Due Date. |

**CARD views in this example:**
- Create Work Order — streamlined creation form
- Work Order — full detail view
- Procedure based Work Order — procedure-focused
- External Work Order — internal view of external submissions
- External Work Order Request — PUBLIC form for external users

**Another public CARD view:**
- Public Work Order (READONLY) — public read-only view

### Asset and inventory collections

Two additional saved collections support the dashboard: **Non-Operational Assets** selects Asset Operational Status = Non-Operational; **Out of Stock Parts** selects calculated Parts Quantity ≤ 0. The latter uses the numeric result of the quantity calculation, so it needs no separate Parts Status field or status-writing automation.


### External requests

Use [publish an external request form](../publish-an-external-request-form.md) for the public Card view and link. This example chooses a triage state, origin Tag, requester fields, and email loop:

View: "External Work Order Request" (public CARD)

**What external users see:**
- Title, File upload, Notes (primary input)
- Requester Name, Phone, Email (in a "Requester Information" section)
- Location, Asset (in an "Additional Information" section)

**What's hidden from external users (nearly everything else):**
- Status (overridden to default "Pending Review" via view override)
- Priority, Assignee, Due Date, Scheduling
- Category, Parts, Vendors, Meters, Checklists
- All internal tracking fields, REFERENCED_INs, INPUT_BUTTONs

**Key override on Status:** Hidden from the external user, readonly, with a default value of "Pending Review" instead of the normal "Open." This gives dispatchers a triage step — the workflow can triage submissions before they enter its ordinary work queue; verify the submitted record.

**Source override:** Hidden and readonly, with a default option labeled "External Form" (value `"external"`). This is intended to tag external submissions for the email notification automation; verify the submitted Source value before relying on that condition (it checks Source = "external" as its condition).

### Email notification loop

When an external WO is created with Source = "external" and Requester Email populated:
1. ENTITY_CREATED automation checks Source = "external"
2. SEND_EMAIL action uses `recipientsFromEmailComponentIds` targeting the Requester Email field
3. External requester receives a confirmation email

When the WO status changes (e.g., to Complete):
1. ENTITY_UPDATED automation checks Source = "external" AND status transition
2. SEND_EMAIL to the requester with the update


## Card layouts

Use [arrange card forms](../arrange-card-forms.md) for `SINGLE_COMPONENT` and `SECTION` construction, layout replacement, and rendering checks. These three layouts illustrate how creation, detail, and external submission differ:

### Card views by audience

The example work-order workflow has several CARD views serving different audiences:

| Card View | Audience | Layout Style |
|---|---|---|
| Create Work Order | Internal creators | Streamlined, separate sections per domain |
| Work Order (detail) | Managers/technicians | Dense, consolidated 2-col details section |
| External WO Request | External submitters | Minimal, requester-focused |
| Procedure WO | Procedure-focused users | Default (no explicit layout — component order) |
| External Work Order | Internal review of external | Default (no explicit layout) |
| Public Work Order | Public readonly | Default (no explicit layout) |

Not every CARD view needs a custom layout. Configure one when the default field order obstructs the intended task.


### Create Work Order — Streamlined creation form

```
SINGLE_COMPONENT: Title (name)
SINGLE_COMPONENT: Status
SINGLE_COMPONENT: Notes
SINGLE_COMPONENT: File
SINGLE_COMPONENT: Priority
SINGLE_COMPONENT: Assignee
SECTION "Schedule and Recurrence" [1-col]: Due Date, Start Date, Reminders
SECTION "Location Details"        [2-col]: Location, Address (lookup)
SECTION "Asset Details"           [2-col]: Asset, Asset Op Status
SINGLE_COMPONENT: Category
SINGLE_COMPONENT: Parts
SINGLE_COMPONENT: Vendors
SINGLE_COMPONENT: Checklist (TODO)
SINGLE_COMPONENT: Procedure (SUBFORM)
SINGLE_COMPONENT: INPUT_BUTTON (Update Status)
SINGLE_COMPONENT: INPUT_BUTTON (Update Asset Op Status)
```

**Design intent:** Primary fields (title, status, notes, files) are standalone at the top for fastest access. Related fields are grouped into sections — scheduling together, location pair together, asset pair together. INPUT_BUTTONs are at the bottom since they're less relevant during creation.

### Work Order detail — Full detail view for managing an active WO

```
SINGLE_COMPONENT: Title (name)
SINGLE_COMPONENT: Status
SINGLE_COMPONENT: Notes
SINGLE_COMPONENT: File
SECTION "Work Order Details" [2-col]: Created On, WO ID, Priority, Category,
                                       Location, Address (lookup), Asset, Asset Op Status
SINGLE_COMPONENT: Assignee
SECTION "Schedule and Recurrence" [1-col]: Due Date, Start Date, Reminders
SINGLE_COMPONENT: Vendor
SECTION "Procedures" [1-col]: Procedure (SUBFORM), Checklist (TODO)
SECTION "Time and Cost Tracking" [1-col]: Est. Duration, Est. Cost, Parts,
                                           Costs REFERENCED_IN, Time Tracking REFERENCED_IN
SINGLE_COMPONENT: Downtime REFERENCED_IN
SINGLE_COMPONENT: INPUT_BUTTON (Update Status)
SINGLE_COMPONENT: INPUT_BUTTON (Update Asset Op Status)
SINGLE_COMPONENT: Meters
SINGLE_COMPONENT: Source
```

**Design intent:** The detail view consolidates metadata into a dense 2-column "Work Order Details" section. REFERENCED_IN components are grouped with their domain (costs and time tracking together). The layout shows more fields than the creation form, including Source and Meters.

### External Work Order Request — Minimal public form

```
SINGLE_COMPONENT: Title (name)
SINGLE_COMPONENT: File
SINGLE_COMPONENT: Notes
SECTION "Requester Information" [2-col]: Requester Name, Requester Phone, Requester Email
SECTION "Additional Information" [2-col]: Location, Asset
```

**Design intent:** External users see only what they need. No status, no priority, no internal fields. Requester info is grouped into a labeled section so they understand what's expected. Location and Asset are optional context.


## List rows

Use [show and edit Tags in lists](../show-and-edit-tags-in-lists.md) to decide which Tags to summarize and which to expose as quick actions. This configuration combines Status, Priority, Category, and Asset Operational Status and adds buttons for frequent field edits:

### Combined status strip

The WO template has a single COMBINED_TAGS component labeled "Tags" that merges four TAG fields into one strip. The four fields being combined:

| Field | Example Values |
|---|---|
| Asset Operational Status | 🟢 Operational, ⚠️ Partially Operational, 🛑 Non-Operational |
| Work Order Status | Pending Review, Open, In Progress, On Hold, Complete |
| Category | Damage, Electrical, Inspection, PM, Safety, Mechanical |
| Priority | None, Low, Medium, High |

### The Complementary Show/Hide Pattern

This example uses COMBINED_TAGS in List views for scanning and shows the individual Tags in internal Card views where people edit them. The public request form shows neither: its internal status, priority, category, and asset-condition fields are hidden from submitters.

| View | COMBINED_TAGS | Individual TAGs (status, priority, category, asset op status) |
|---|---|---|
| **LIST views** (Status, Priority, Location, Assigned to me, Overdue) | ✅ Shown | ❌ Hidden |
| **External WO Request** (public CARD) | ❌ Hidden | ❌ Hidden |
| **CARD views** (Create WO, Work Order, Procedure WO, External WO) | ❌ Hidden | ✅ Shown |
| **BOARD view** | ❌ Hidden | ❌ Hidden (status implied by column) |
| **CALENDAR views** | ❌ Hidden | ❌ Hidden |

### Why This Split?

On an editable work-order Card, separate Tag fields let a person change Status, Priority, and Category independently. Showing the combined display as well may repeat the same information; a read-only summary can justify a different choice.

**LIST views are for scanning.** When a technician scrolls through their work order list, they need maximum information density per row. Showing four separate TAG fields would eat up the entire row width. COMBINED_TAGS collapses them into a single compact strip:

```
[WO-1042: Fix HVAC Unit 3] [🟢 Operational | Open | Mechanical | High] [Update Status ▾]
```

vs. without COMBINED_TAGS (four separate columns):

```
[WO-1042] [Open] [High] [Mechanical] [🟢 Operational]  ← too wide, no room for other fields
```

This example's Board hides both because its grouping already communicates the primary Status value. Other Tags may still be useful on a Board card.

The public form omits the combined display because it would expose classifications the submitter does not need. A different public view could show a selected summary when that information helps its audience; decide from the intended disclosure and task.

### Quick actions

The Work Orders template has two INPUT_BUTTONs:

| Component ID | Button Text | Target Field | Target Field Label |
|---|---|---|---|
| — | "Update Status" | status TAG | Work Order Status |
| — | "Update Asset Operational Status" | asset op status TAG | Asset Operational Status |

### View-by-View Visibility

This work-order configuration chooses where each button appears. Use it as an example of audience-specific placement, not a visibility rule for all Coast views:

| View | Type/Subtype | "Update Status" | "Update Asset Op Status" |
|---|---|---|---|
| **Status List** | COLLECTION/LIST | ✅ Shown | ✅ Shown |
| **Priority List** | COLLECTION/LIST | ✅ Shown | ✅ Shown |
| **Location List** | COLLECTION/LIST | ✅ Shown | ✅ Shown |
| **Assigned to me** | COLLECTION/LIST | ✅ Shown | ✅ Shown |
| **Overdue Work Orders** | COLLECTION/LIST | ✅ Shown | ✅ Shown |
| **Board View** | COLLECTION/BOARD | ❌ Hidden | ❌ Hidden |
| **Table View** | COLLECTION/TABLE | Inherited visible | Inherited visible |
| **PM List View** | COLLECTION/LIST | ❌ Hidden | ❌ Hidden |
| **Calendar View** | COLLECTION/CALENDAR | ❌ Hidden | ❌ Hidden |
| **PM Calendar View** | COLLECTION/CALENDAR | ❌ Hidden | ❌ Hidden |
| **Create Work Order** | CARD | Inherited visible | Inherited visible |
| **Work Order** | CARD | Inherited visible | Inherited visible |
| **External WO Request** | CARD (public) | ❌ Hidden | ❌ Hidden |
| **External Work Order** | CARD | Inherited visible | Inherited visible |
| **Procedure WO** | CARD | Inherited visible | Inherited visible |

### Why This Distribution?

In this workflow, technicians use a List view to scan assigned work and update status quickly. The INPUT_BUTTON lets them:
- Tap "Update Status" → change from "Open" to "In Progress" → keep scanning
- Tap "Update Asset Operational Status" → mark asset as "Non-Operational" → move on

Without INPUT_BUTTONs, the technician would need to: tap the card → scroll to the field → tap the field → select the value → close the card. That's 4+ taps vs. 2 taps.

On a Board where changing the grouped Tag is already supported, the same button may be redundant. Check the connected client's Board interaction.

On a Calendar used only for planning, the button may add clutter. A scheduling workflow that needs quick status edits can make a different choice.

The example PM List is a planning view that includes upcoming occurrences, so it omits quick-edit buttons. Technicians change a PM's Status in the editable Work Order detail Card or an operational List with **Update Status**. The status automations below react to those edits; they do not advance Status themselves.


## Automations

Use [author automations](../author-automations.md) for trigger and condition mechanics; use [sync related records on status change](../sync-related-record-on-status-change.md) for choosing the related-record action. The automations below are an example composition, not Coast's default maintenance policy.

### Bridge Automations (ENTITY_CREATED)

These two bridge rules use `suppressAutomationChain: true`: their writes maintain internal selection fields and should not trigger other work-order update rules. The timer-start rules below must allow chaining so the child-creation events can reach these bridges.

- **"Set Downtime"** — Downtime Tracking: On create, follows its **Service Request** RELATED_CARD to set the WO's hidden **Current Downtime** RELATED_CARD to this Downtime entity as its one intended current child. A later child replaces that selection; the bridge does not collect every child. See [related-record traversal recipe](../related-record-display-and-traversal.md) for the one-child limit and reconciliation choices.
- **"Set Current Labor Entry"** — Time Tracking: On create, follows the entry's **Work Order** RELATED_CARD and writes this entry into that work order's hidden **Current Labor Entry** RELATED_CARD. The reverse **Time Tracking** display still shows all entries; this stored bridge selects only the latest one for parent-side actions. Restrict creation of another running entry until the current one is stopped, since replacing the bridge cannot stop the former entry.

### Status-Driven Cross-Workspace Updates (ENTITY_UPDATED)

Set `suppressAutomationChain: false` on both timer-start rules. Suppressing their child-creation events would leave the work order's current-child field unset or stale, so a later stop rule could not target the new Timer.

- **"PM - Start Asset Downtime Timer"** — WOs: A transition into **In Progress** with Category PM creates a Downtime record, sets its **Service Request** RELATED_CARD to the triggering WO, and starts its Timer with `TIME_TRACKER_DATA_ACTION` / `START`. The child-side bridge selects it as **Current Downtime**.
- **"PM - Stop Asset Downtime Timer"** — WOs: A transition out of **In Progress** with Category PM updates **Current Downtime** through `UPDATE_WORKFLOW_ENTITY`, writing `TIME_TRACKER_DATA_ACTION` / `STOP` to that record's Timer. This includes a move to **On Hold**, **Open**, or **Complete**, not only completion.
- **"WO - Start Work Order Timer"** — WOs: A transition into **In Progress** with a selected non-PM Category creates one **Time Tracking** record, sets its **Work Order** RELATED_CARD to the triggering WO, and starts **Labor Timer** with `TIME_TRACKER_DATA_ACTION` / `START`. The child-side bridge selects it as **Current Labor Entry**.
- **"WO - Stop Work Order Timer"** — WOs: A transition out of **In Progress** with a selected non-PM Category uses `UPDATE_WORKFLOW_ENTITY` through **Current Labor Entry** to write `TIME_TRACKER_DATA_ACTION` / `STOP` to **Labor Timer**. The [status-change recipe](../sync-related-record-on-status-change.md) shows the action and field-value structure.
- **"Create/Update WO - Asset Non/Partially/Operational"** — WOs: Six automations (3 on ENTITY_CREATED, 3 on ENTITY_UPDATED) that sync Asset operational status based on a TAG field. Paired create+update automations ensure the sync fires both on initial WO creation and on later edits.

This example starts ordinary work at **Open** (external requests first pass through **Pending Review**) and selects Category before moving to **In Progress**. Match the configured non-PM option values explicitly in that branch; an `IS_NONE_OF` PM condition alone may also admit an unset Category. These rules compare current and previous Status, so an unrelated edit while Status stays **In Progress** does not create another child. Keep Category fixed during an active interval. Before a later return to **In Progress**, check that the prior selected Timer stopped and that no other child is running; then the new entry becomes current while the reverse display retains the history. Verify each child's bridge write before the next status change. The one-current-child fields do not enforce exclusivity for manually created or reassigned children; reconcile those paths before enabling them. The [related-record traversal recipe](../related-record-display-and-traversal.md) explains that limit.

### Notification Automations

- **"Email Notifications on External Work Order Create/Update"** — WOs: Fires email when Source tag = "external", emails the requester.
- **"Send Reminder"** — WOs (ADHOC): The top-level eligibility condition excludes Status **Complete** (the configured Tag value `"complete"`, compared as `["complete"]`), so scheduled reminders do not notify about completed work. Its two push actions retain their own Due Date conditions: "due soon" when Due Date > NOW and "overdue" when Due Date < NOW. The Reminders component schedules this rule relative to Due Date. This completed-work exclusion is this example's policy, not a default for all reminders.

### Recurring queue visibility

**Make Recurring Card Visible** is the enabled `ADHOC` rule used by the [recurring queue configuration](#recurring-work-in-operational-queues). Its Scheduled Automation invocation updates the current record's Visible Tag; the operational collections then include it when their other criteria match. This rule does not generate the recurring series or deliver the reminder notification.

### Calculation Chains (ADHOC)

- **"Update Parts Quantities"** — Parts Inventory: sum recorded usage quantities → subtract that sum from the initial quantity → write the result to the on-hand field. This three-action example can be invoked from a Work Order through `TRIGGER_RELATED_CARD_AUTOMATION`. The [quantity recipe](../track-consumed-quantities.md) explains the write, and why usage edits or removed Part links need additional handling.

### Cross-Workspace Trigger Chains

- **"Card Created/Updated: Recalculate parts quantities"** — WOs: Both fire TRIGGER_RELATED_CARD_AUTOMATION against the Parts Inventory ADHOC automation whenever WO parts change.


## Dashboard

Use [add dashboard widgets](../add-dashboard-widgets.md) after creating the saved collections defined in [Views and forms](#views-and-forms). Each widget uses its backing view's criteria; no filter is defined again on the widget. Resolve the workspace, active template, and view IDs before adding it.

This example has six widgets spanning three workspaces:

| Widget | Workspace | Backing saved collection |
|---|---|---|
| Overdue Work Orders | Work Orders & PMs | Overdue Work Orders |
| My Work Orders | Work Orders & PMs | Assigned to me |
| PM List | Work Orders & PMs | PM List View |
| Non-Operational Assets | Asset Management | Non-Operational Assets |
| Out of Stock Parts | Parts Inventory | Out of Stock Parts |
| All Work Orders and PMs | Work Orders & PMs | Table View |

### Design Choices

**Action-oriented names.** "Overdue Work Orders" tells you what needs attention, not "Work Orders by Date." "Out of Stock Parts" signals a problem.

**Cross-workspace coverage.** Widgets span WOs, Assets, and Parts — the three areas a maintenance manager monitors daily.

**Personal + team views.** "My Work Orders" (current-user filter) gives individuals their queue. "Overdue Work Orders" gives managers the global picture.

**Count as KPI.** Interpret each count using its backing collection. Overdue and out-of-stock counts indicate operational work; **PM List** and **All Work Orders and PMs** also count upcoming recurring records because their planning collections include them.
