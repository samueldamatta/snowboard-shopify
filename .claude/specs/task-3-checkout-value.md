# SPEC — Task 3: Add value to checkout without extra cost (Plus)

## Goal
The client is on **Shopify Plus** and wants to **add value to the checkout without incurring more
expenses** (i.e. no paid third-party apps). Deliverable: a recommendation document, optionally with a
scaffolded Checkout UI extension as a demo.

## Decision
Recommend only **free/native** Plus capabilities built on **Checkout Extensibility** (note:
`checkout.liquid` is sunset — extensions are the path forward). Deliver as
`docs/checkout-recommendations.md` in the repo.

## Recommendations (all free for Plus, built in-house)
- **Checkout UI extensions** — trust badges, delivery/returns reassurance, gift-message field, custom
  banners, in-checkout product offers. No app subscription.
- **Shopify Functions** (free):
  - *Discounts* — free gift / tiered / BOGO without a paid discount app.
  - *Bundles / volume discounts*.
  - *Payment customizations* — e.g. hide COD above a threshold.
  - *Delivery customizations* — rename/reorder/hide shipping methods.
- **Post-purchase checkout extension** — one-click upsell on the post-purchase page; lifts AOV without
  touching checkout conversion.
- **Thank-you / Order-status page extensions** — cross-sell, NPS/survey, referral prompts.
- **Checkout Branding API** (Plus) — brand-match checkout for trust/conversion.
- **Free-shipping progress bar / cart goal** via cart + checkout UI extension.
- **Avoid** paid checkout/upsell apps (Rebuy, ReConvert, etc.) given the no-cost constraint.

## Optional concrete artifact
Scaffold a minimal Checkout UI extension to demonstrate the recommendation:
`shopify app generate extension` → choose *Checkout UI* (e.g. trust-badge or free-shipping bar).

## Files / deliverables
- `docs/checkout-recommendations.md` (new) — the written recommendation.
- (optional) `extensions/<name>/` — scaffolded Checkout UI extension demo.

## Acceptance criteria
- Document clearly lists native, no-extra-cost options mapped to "added checkout value".
- Each item notes why it's free for Plus and what value it adds (AOV, trust, conversion).
- Any code artifact runs via `shopify app dev` and renders in checkout preview.

## Verification
Review the doc; if an extension is scaffolded, run `shopify app dev` and confirm it renders in the
checkout preview.
