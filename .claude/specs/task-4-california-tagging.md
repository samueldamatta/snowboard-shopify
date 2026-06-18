# SPEC — Task 4: Auto-tag California customers as high-value

## Goal
Automatically **tag customers who purchase from California** as **high-value customers**.

## Decision
Use **Shopify Flow** (free for Plus). No code in the theme — this is an admin automation, documented
in the repo with an export/screenshot.

## Flow workflow
- **Trigger:** *Order created* (use *Order paid* if it should fire only after payment).
- **Condition:** shipping address province is California —
  `order.shippingAddress.provinceCode == "CA"` (or province name equals `California`).
- **Action:** *Add customer tags* → e.g. `High Value Customer` (and/or `California VIP`).

### Optional refinement
If "high value" should also depend on spend, add a second condition, e.g.
`order.currentTotalPriceSet.shopMoney.amount` greater than a threshold, combined with the province check.

## Deliverables
- `docs/flow-california-high-value.md` (new) — description + exported workflow JSON / screenshot.

## Open items
- Confirm the **exact tag string** the client wants (`High Value Customer` vs `VIP`, etc.).
- Confirm trigger timing (order created vs paid) and whether province uses billing or shipping address.

## Acceptance criteria
- A Flow workflow exists that tags customers whose order ships from California.
- The tag is verifiable on a test order's customer record.
- Workflow is documented (export/screenshot) in the repo.

## Verification
In Flow, run/test the workflow against a California test order and confirm the customer receives the tag.
