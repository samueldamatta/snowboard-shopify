# SPEC — Task 1: Icon swatches on the variant picker

## Goal
The client wants the variant picker on the product page to display **icon swatches** instead of
plain text option pills.

## Decision
Use **native Shopify swatches** (the `value.swatch` Liquid object). Icons are represented as
**image swatches** assigned per option value in the admin. Reuse Dawn 15's swatch pattern, which
exists locally at `/Users/samueldamatta/Documents/ProjetosWEB/lumae-skincare` (Dawn 15.4.1).

## Current state (samu-test)
- Theme is Dawn-based, **button-style** picker (`variant-radios`).
- `snippets/product-variant-options.liquid` renders only `<input type="radio">` + `<label>{{ value }}` — **no swatch markup**.
- No `snippets/swatch.liquid`, no swatch CSS, no swatch settings.

## Admin steps (Samuel)
1. Open *The Complete Snowboard* → edit the **Color** option.
2. For each value (Ice, Dawn, Powder, Electric, Sunset) assign a **swatch image** (the icon PNG/SVG).
   Image swatches are how "icon" swatches are exposed to Liquid via `value.swatch.image`.

## Code changes (samu-test)
1. **Add `snippets/swatch.liquid`** — port from `lumae-skincare/snippets/swatch.liquid`.
   Renders a `.swatch` span using `--swatch--background`: `url(...)` when `swatch.image`, else
   `rgb(...)` when `swatch.color`.
2. **Edit `snippets/product-variant-options.liquid`** (model on Dawn 15's version):
   - compute `swatch_count = option.values | map: 'swatch' | compact | size`;
   - when a value has a `swatch`, render `{% render 'swatch', swatch: value.swatch, shape: ... %}`
     inside the button `<label>` and as a marker for the dropdown;
   - keep existing `option_disabled` logic and the `visually-hidden` value text for a11y.
3. **Add swatch CSS** — new `assets/component-swatch.css` (port Dawn 15 swatch / `swatch-input`
   styles), loaded from `sections/main-product.liquid` via `stylesheet_tag`. Style swatch as an icon
   chip: shape (square/round), selected-state ring, disabled diagonal line.
4. **Add a `swatch_shape` setting** to the `variant_picker` block schema in
   `sections/main-product.liquid` (`circle` / `square` / `none`) to toggle icon swatches vs old pills.

## Files touched
- `snippets/swatch.liquid` (new)
- `snippets/product-variant-options.liquid`
- `assets/component-swatch.css` (new) + load in `sections/main-product.liquid`
- `sections/main-product.liquid` (block schema setting)

## Reference to reuse
- `lumae-skincare/snippets/swatch.liquid`
- `lumae-skincare/snippets/product-variant-options.liquid`
- `lumae-skincare/assets/component-swatch.css`

## Acceptance criteria
- Color picker shows 5 icon swatches on the PDP.
- Selected, hover, and disabled/sold-out states render correctly.
- Keyboard navigation + screen-reader value text preserved.
- No console errors; existing add-to-cart / variant selection still works.

## Verification
Open `http://127.0.0.1:9292/products/the-complete-snowboard` with `shopify theme dev` running;
confirm icons render and selection state updates.
