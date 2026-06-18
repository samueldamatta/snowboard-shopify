# Adding Value to Checkout — No Extra Cost (Shopify Plus)

**Audience:** a Shopify **Plus** merchant who wants to add value to checkout **without paid
third-party apps**.

**TL;DR:** Everything below is **native to Plus and free** (no app subscription), built in-house on
**Checkout Extensibility**. The legacy `checkout.liquid` is **sunset** — UI extensions and Shopify
Functions are the supported path forward. Skip paid upsell/checkout apps (Rebuy, ReConvert, etc.); the
same outcomes are achievable natively for $0.

---

## Why these are free for Plus

- **Checkout UI extensions, post-purchase, and Thank-you / Order-status extensions** are part of
  Checkout Extensibility — included with the platform. Building them yourself costs only dev time, no
  recurring license.
- **Shopify Functions** (discounts, payment/delivery customizations) run on Shopify's infrastructure
  at no per-use cost.
- **Checkout Branding API** and full **checkout customization** (multiple custom UI extensions,
  branding control) are **Plus-only** entitlements — you're already paying for them in the Plus plan.
- **No `checkout.liquid`:** it's deprecated/sunset, so there's nothing to migrate *to* except
  extensions. That makes the native path the only forward-compatible (and cheapest) one.

---

## Recommendations mapped to "added checkout value"

Each item lists **what to build**, **why it's free for Plus**, and the **value** it adds
(AOV = average order value, Trust, Conversion).

### 1. Checkout UI extensions
Native UI blocks injected at defined checkout extension points.

| Build | Why free for Plus | Value |
|---|---|---|
| **Trust badges** (secure-payment, money-back, SSL) near payment | Native UI extension, no app | **Trust** → fewer drop-offs at payment |
| **Delivery & returns reassurance** banner | Native UI extension | **Conversion** — removes hesitation |
| **Gift-message field** (custom field saved to the order) | `Checkout::*` extension + metafield/attribute | **AOV / experience** — captures gifting demand |
| **In-checkout product offer** (small add-on / warranty / gift wrap) | Native UI extension reading cart | **AOV** — adds units without leaving checkout |
| **Custom banners** (promo, shipping cutoff, free-gift threshold messaging) | Native UI extension | **Conversion / AOV** |

> Plus allows **multiple** custom checkout UI extensions and broader extension points than non-Plus
> plans — exploit that to place several of the above.

### 2. Shopify Functions (free, self-hosted logic)

| Build | Why free for Plus | Value |
|---|---|---|
| **Discounts Function** — free gift, tiered, BOGO | Replaces a paid discount app | **AOV** — incentivizes larger carts |
| **Bundles / volume discounts** | Native Function | **AOV** |
| **Payment customizations** — e.g. hide COD above a threshold, reorder methods | Native Function | **Trust / risk** — fewer bad orders |
| **Delivery customizations** — rename / reorder / hide shipping methods | Native Function | **Conversion** — clearer choices reduce friction |

### 3. Post-purchase checkout extension
One-click upsell on the **post-purchase page** (after payment, before Thank-you).

- **Why free:** native post-purchase extension surface — no upsell-app subscription.
- **Value:** **AOV** — lifts order value **without touching checkout conversion**, since the customer
  has already paid. Lowest-risk upsell surface available.

### 4. Thank-you / Order-status page extensions
UI extensions on the Thank-you and Order-status pages.

- **Cross-sell** recommended products, **NPS / survey**, **referral** prompt, social follow.
- **Why free:** native extension surfaces (these replaced the old Order-status `additional scripts`).
- **Value:** **AOV** (repeat / cross-sell) + **retention** (feedback, referrals) with **zero**
  conversion risk.

### 5. Checkout Branding API (Plus only)
Programmatically brand-match the checkout — colors, typography, corner radius, logo, layout.

- **Why free:** a Plus entitlement; no app needed.
- **Value:** **Trust / Conversion** — a checkout that visually matches the storefront reduces "is this
  the same store?" anxiety and cart abandonment.

### 6. Free-shipping progress bar / cart goal
A "spend $X more for free shipping" indicator, surfaced via a **cart + checkout UI extension** and
backed by a free-shipping **Discounts Function**.

- **Why free:** cart/checkout UI extension + native Function.
- **Value:** **AOV** — one of the highest-ROI nudges to grow basket size.

---

## What to avoid

- **Paid checkout/upsell apps** — Rebuy, ReConvert, AfterSell, etc. Every capability above is
  achievable natively for $0 on Plus; a subscription adds recurring cost with no unique capability for
  this scope.
- **`checkout.liquid` and Order-status `additional scripts`** — **sunset**. Don't invest there; build
  on extensions.

---

## Suggested rollout order (impact ÷ effort)

1. **Free-shipping progress bar** + **Discounts Function** — biggest AOV lever, low effort.
2. **Trust badges + reassurance banner** (Checkout UI) — quick Trust/Conversion win.
3. **Post-purchase one-click upsell** — AOV with zero conversion risk.
4. **Thank-you cross-sell + NPS/referral** — AOV + retention.
5. **Checkout Branding API** — polish pass for Trust/Conversion.
6. **Payment/Delivery customizations** as needed.

---

## Optional concrete artifact — scaffold a demo Checkout UI extension

The extension demo lives in a **Shopify app**, not in this theme repo. To scaffold it:

```bash
# from a Shopify app project (create one with `shopify app init` if needed)
shopify app generate extension
# → choose: Checkout UI
# → name it e.g. trust-badge or free-shipping-bar
shopify app dev   # renders in the checkout preview
```

A minimal trust-badge extension (`src/Checkout.jsx`) looks like:

```jsx
import {
  reactExtension,
  Banner,
  BlockStack,
  Text,
} from "@shopify/ui-extensions-react/checkout";

export default reactExtension("purchase.checkout.block.render", () => <Extension />);

function Extension() {
  return (
    <BlockStack spacing="tight">
      <Banner status="info" title="Shop with confidence">
        <Text>Secure SSL checkout · 30-day returns · Money-back guarantee</Text>
      </Banner>
    </BlockStack>
  );
}
```

…with the matching `shopify.extension.toml` declaring an extension of `type = "ui_extension"` and a
`purchase.checkout.block.render` target. Place it via the checkout editor, then verify with
`shopify app dev`.

---

## Acceptance criteria check

- ✅ Lists **native, no-extra-cost** options mapped to "added checkout value".
- ✅ Each item notes **why it's free for Plus** and **what value** it adds (AOV / Trust / Conversion).
- ✅ Optional code artifact provided with exact `shopify app dev` run instructions and renderable
  source.
