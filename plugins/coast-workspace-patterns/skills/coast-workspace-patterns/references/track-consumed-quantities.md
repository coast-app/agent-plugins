# Track remaining quantity from recorded usage

## What It Does

This recipe calculates remaining stock from quantities recorded on relationships. A Work Order can record how many Parts it uses, while a Part record can hold initial stock and an updated on-hand value. Those fields and writes are configured for the particular inventory process; Coast does not supply a universal stock policy.

The consuming template's `RELATED_CARD` needs `hasQuantity: true` to store a quantity with each selected Part. An automation can aggregate those quantities and write a result to the Part. Decide first whether receiving, returns, reservations, and edits to prior usage must also affect the balance.

## The Mechanism

### hasQuantity on the RELATED_CARD

The example Work Orders template has a Parts RELATED_CARD with `hasQuantity: true`. Generate a fresh UUID for a new component ID:

```json
{
  "id": "<new-component-uuid>",
  "type": "RELATED_CARD",
  "label": "Parts",
  "hasQuantity": true,
  "workflowTemplateId": "<parts-inventory-template-id>"
}
```

When `hasQuantity: true`, the RELATED_CARD picker lets users specify a quantity for each linked entity.

### The Computation Chain

On the Parts Inventory workspace, an ADHOC automation ("Update Parts Quantities") is triggered by WOs whenever parts quantities change. The chain:

1. **REFERENCED_IN_QUANTITY_SUM** — Produces the sum of quantities from matching Work Orders that reference this Part; it does not write a field itself.
2. **CALCULATE_VALUE** — Computes Initial Quantity minus that sum.
3. **UPDATE_CURRENT_WORKFLOW_ENTITY** — Writes the computed result to the Parts Quantity field

The optional named-intermediate `+ 0` calculation and its JSON shape are described in [author automations](author-automations.md#calculate_value).

## How To Build It

### Step 1: Set hasQuantity on the consuming template's RELATED_CARD

On Work Orders (or whatever template consumes inventory):
```json
{ "id": "<new-component-uuid>", "type": "RELATED_CARD", "hasQuantity": true, "workflowTemplateId": "<parts-template-id>" }
```

### Step 2: Add Initial Quantity and computed Parts Quantity on the inventory template

- Initial Quantity: editable NUMBER field (user sets when stocking)
- Parts Quantity: readonly NUMBER field (computed by automation)

Treat the calculated balance as automation-managed and protect it from ordinary manual edits through the intended view and access rules. Read-only presentation alone is not complete write authorization.

### Step 3: Create the ADHOC automation on the inventory template

Actions in order:
1. REFERENCED_IN_QUANTITY_SUM — sums quantities from all referencing WOs
2. CALCULATE_VALUE (Initial - Consumed) — reads the initial field and the sum action result.
3. UPDATE_CURRENT_WORKFLOW_ENTITY — writes the result to Parts Quantity.

### Step 4: Trigger from the consuming workspace

On Work Orders, create an automation that invokes the Parts ADHOC automation when relevant usage changes. Account for removals, reassignment, and repeated events so the old and new Part balances both stay correct; a rule that only follows currently selected Parts can miss a removed Part.

## Verify the balance

Enable the target ADHOC automation before the consuming rule invokes it. Test the full write path with a representative Part: add usage, change its quantity, and remove or reassign the Part. Inspect the affected old and new balances after each edit.
