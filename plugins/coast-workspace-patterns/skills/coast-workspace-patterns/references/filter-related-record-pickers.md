# Filter a related-record picker by another field

## What It Does

Filters a RELATED_CARD picker based on a value already selected elsewhere on the same card. When the user picks a value in field A, the picker for field B automatically narrows to only show entities whose matching field equals field A's current value.

Dynamic filtering is opt-in through the Related Card field's `listDefault.filterByComponents`. Without this criterion, the picker follows its other configured criteria, search, and access rules; it does not necessarily show every target record.

## Decide whether to narrow the picker

| Scenario | Decision | Why |
|---|---|---|
| Downtime → Asset, scoped to Location | Apply when required | Use when this downtime process requires the asset to match the selected location. |
| WO → Asset, any location | Omit when cross-location selection is allowed | Omit the filter if this work-order process permits cross-location assets. |
| WO → Parts, scoped to Asset | Apply only with asset-specific stock | Only if parts are asset-specific, not shared |
| Invoice → Vendor, scoped to Region | Apply when region is required | If vendors serve specific regions |

## Prerequisites

For this pattern to work, three things must be true:

1. **Shared reference workspace** — Both the source template (Downtime) and the target template (Assets) must have RELATED_CARD fields pointing to the same third template (Locations).

2. **Current value matters** — The source field (Location) must hold a value before this dynamic criterion can narrow the target picker. The web client omits a dynamic criterion whose source value is empty or absent, so other criteria still apply. Verify another client's behavior when that interaction matters.

3. **Matching field IDs** — The `componentId` in the filter must be the actual field ID on the TARGET template (e.g., `location` on Asset Management), and the `containsCurrentWorkflowTemplateComponentValue.componentId` must be the actual field ID on the SOURCE template (the Location field on Downtime Tracking).

## Example configuration

### Example: Downtime Tracking → Asset filtered by Location

Downtime Tracking has two RELATED_CARDs:
- **Location** → points to Locations template
- **Asset** → points to Asset Management template

Asset Management also has a **Location** field → points to the same Locations template.

The Asset RELATED_CARD is configured with a dynamic filter. IDs below illustrate the wiring; replace new component IDs with UUIDs from `generate_uuid` and template references with the actual IDs returned by Coast:

```json
{
  "id": "<new-component-uuid>",
  "type": "RELATED_CARD",
  "label": "Asset",
  "workflowTemplateId": "<asset-management-template-id>",
  "listDefault": {
      "filterByComponents": [
        {
          "componentId": "<location-field-id-on-ASSET-template>",
          "value": {
            "containsCurrentWorkflowTemplateComponentValue": {
              "componentId": "<location-field-id-on-this-template>"
            }
          }
        }
      ]
  }
}
```

Reading this config:

| Config Key | Points To | Meaning |
|---|---|---|
| `workflowTemplateId` | Asset Management template | "This picker shows Asset entities" |
| `filterByComponents[0].componentId` | Location field on Asset Management | "Filter Assets by their Location field" |
| `containsCurrentWorkflowTemplateComponentValue.componentId` | Location field (on Downtime Tracking) | "...where it matches whatever Location is selected on THIS Downtime card" |

### User Experience

1. User creates a new Downtime record
2. User picks **Location = "Building A"** in the Location field
3. User taps the **Asset** picker — it only shows assets where `asset.location == "Building A"`
4. If the user changes Location to "Building B", the Asset picker re-filters automatically

### Design Choice: Work Orders Does NOT Use This

Work Orders also has both Location and Asset fields, but its Asset picker has **empty** `filterByComponents`:

```json
{
"listDefault": {
  "filterByComponents": [],
  "sortByComponents": []
}
}
```

This illustrates a different product choice. A work-order process may allow an asset outside the currently selected location, while a location-specific downtime process may require a narrower picker. Choose the filter from the customer's actual relationship rules.

**Takeaway:** Don't apply dynamic filtering by default. Apply it only when the business relationship genuinely requires it.

## How To Build It

### Step 1: Ensure the shared reference exists

Both templates must have RELATED_CARD fields pointing to the same target. Example:
- Template A (Downtime) has Location → Locations template
- Template B (Assets) has Location → Locations template

### Step 2: Add the filter to the child RELATED_CARD

When creating or updating the "child" RELATED_CARD (Asset on Downtime), set its top-level `listDefault`:

```json
{
  "id": "<new-component-uuid>",
  "type": "RELATED_CARD",
  "placeholder": "Select Asset",
  "workflowTemplateId": "<asset-management-template-id>",
  "hasQuantity": false,
  "listDefault": {
      "filterByComponents": [
        {
          "componentId": "<location-field-id-on-ASSET-template>",
          "value": {
            "containsCurrentWorkflowTemplateComponentValue": {
              "componentId": "<location-field-id-on-THIS-template>"
            }
          }
        }
      ]
  }
}
```

### Step 3: Validate the wiring

- The outer `componentId` is on the **target** template (the one being filtered)
- The inner `componentId` is on the **current** template (the one the user is editing)
- Both fields must reference the same underlying template for the match to work

## Scope Limitation

This dynamic filter is a **picker-time filter**, not a data-time filter. It only works in the RELATED_CARD picker context — it cannot be used in view filters or automation conditions.

## Common Mistakes

| Mistake | What Happens |
|---|---|
| Swapping inner/outer componentIds | Filter matches wrong field — shows unrelated entities or nothing |
| Using field ID from wrong template | The criterion may fail to narrow the picker as intended; inspect the result. |
| Expecting an empty Location to mean no candidates | The web client omits that dynamic criterion when its source is empty. |
| Using this in view collectionCriteria | API error — wrapper only valid in RELATED_CARD listDefault |
| Applying to every RELATED_CARD by default | Over-constrains the system — not all relationships need scoping |

The picker criterion is not a write-time relationship constraint. Enforce a required match through the operation's actual write policy; the picker alone does not protect API writes.
