# Snowboard Store — Shopify Theme (Dawn-based)

Customizations for the **samu-test** store, built on Shopify's [Dawn](https://github.com/Shopify/dawn)
reference theme and developed with the **Shopify CLI**.

- **Store:** https://samu-test-store.myshopify.com
- **Store handle:** `samu-test-store`

---

## What was built

| Feature | Where | Notes |
|---|---|---|
| **Icon swatches** on the variant picker | [`snippets/swatch.liquid`](snippets/swatch.liquid), [`snippets/product-variant-options.liquid`](snippets/product-variant-options.liquid), [`assets/component-swatch.css`](assets/component-swatch.css), [`sections/main-product.liquid`](sections/main-product.liquid) | Uses native Shopify swatches (`value.swatch`). Toggle via the `Swatch shape` block setting (circle / square / none). |
| **Per-variant specification table** | [`sections/variant-specs.liquid`](sections/variant-specs.liquid), wired into [`templates/product.json`](templates/product.json) | Server-renders the selected variant's specs (SEO-safe) and swaps values on variant change via Dawn's `variantChange` pub/sub. Specs come from 8 variant metafields in the `specs` namespace. |
| **Checkout value recommendations** (Plus, no extra cost) | [`docs/checkout-recommendations.md`](docs/checkout-recommendations.md) | Native, free-for-Plus options on Checkout Extensibility. |
| **Auto-tag California buyers as high-value** | [`docs/flow-california-high-value.md`](docs/flow-california-high-value.md), [`docs/flow-california-high-value.flow`](docs/flow-california-high-value.flow) | Shopify Flow automation (admin), documented + representative export committed. |

---

## Admin configuration required

The theme code is data-driven — these must be set in the Shopify admin for the features to render/run:

- **Icon swatches:** assign a **swatch image or color** to each value of the product's **Color** option
  (Products → *The Complete Snowboard* → Options). Values without a swatch fall back to text pills.
- **Spec table:** define 8 **variant** metafields (Settings → Custom data → Variants), namespace `specs`,
  type *Single line text*, keys: `board_type, length, flex_rating, profile, core_material, base,
  skill_level, terrain`. Then fill each variant's values (see the table in
  [`task-2-spec-table.md`](.claude/specs/task-2-spec-table.md)).
- **California tagging:** build and enable the workflow in **Admin → Flow** per
  [`docs/flow-california-high-value.md`](docs/flow-california-high-value.md).

---

## Local development

### Prerequisites

- [Shopify CLI](https://shopify.dev/docs/api/shopify-cli) (`shopify version`)
- A staff/collaborator account with theme access to `samu-test-store`

### Run the dev server

```bash
# Live local preview with hot reload at http://127.0.0.1:9292
shopify theme dev --store samu-test-store
```

### Push / pull

```bash
# Push local changes to a theme on the store
shopify theme push

# Pull the live/remote theme into this folder
shopify theme pull

# List themes on the store
shopify theme list
```

> `.shopify/` (local CLI session + pulled `metafields.json`) is git-ignored — it's machine/store-local
> and should not be committed.

---

## GitHub workflow

This repo is connected to GitHub. To deploy from a branch, you can also connect it in
**Admin → Online Store → Themes → Add theme → Connect from GitHub** for branch-based deploys.

```bash
git add -A
git commit -m "feat: ..."
git push
```

---

## Docs

- [`docs/checkout-recommendations.md`](docs/checkout-recommendations.md) — Task 3 recommendations.
- [`docs/flow-california-high-value.md`](docs/flow-california-high-value.md) — Task 4 Flow automation.
- [`.claude/specs/`](.claude/specs/) — original task specs.
