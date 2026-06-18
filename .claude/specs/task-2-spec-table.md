# SPEC — Task 2: Per-variant specification table

## Goal
Add a **specification table** to the product page that matches the Figma design and shows
**different specs per variant** (values assigned at random).

Figma: `https://www.figma.com/design/PfLBVCBBKUquwNALfZHNob/Developer-test?node-id=0-1`

## Decision
- Store specs in **8 variant metafields** under namespace `specs`.
- New `sections/variant-specs.liquid` renders the selected variant **server-side** and embeds a
  per-variant JSON.
- JS swaps the values on variant change, hooking Dawn's variant-change event.

## Step 0 — Figma (blocking for final styling)
Authorize the Figma MCP: run `/mcp` → **claude.ai Figma** → authorize in browser. Then extract exact
tokens (colors, type scale, spacing, card layout) from the node above. **Fallback** if MCP is
unavailable: build from the draft styling (eyebrow + serif heading with red `#a4231f` accent,
4-column card grid) and reconcile against Figma in QA.

## Admin steps (Samuel)
1. Settings → Custom data → **Variants** → add 8 definitions, type *Single line text*, namespace
   `specs`, keys exactly:
   `board_type, length, flex_rating, profile, core_material, base, skill_level, terrain`.
2. Fill each variant's metafields with the random values (Ice/Dawn/Powder/Electric/Sunset). The bulk
   variant editor can add these as columns. Reference values:

| Spec | Ice | Dawn | Powder | Electric | Sunset |
|---|---|---|---|---|---|
| board_type | All-Mountain | Freestyle | Powder | Freeride | Carving |
| length | 155 cm | 152 cm | 159 cm | 161 cm | 156 cm |
| flex_rating | Medium | Soft | Medium-Soft | Stiff | Medium-Stiff |
| profile | Camber | Rocker | Hybrid Rocker | Camber | Flat-to-Rocker |
| core_material | Aspen/Spruce Blend | Poplar Core | Paulownia Core | Bamboo Core | Aspen Core |
| base | Sintered | Extruded | Sintered 4000 | Sintered | Extruded |
| skill_level | Intermediate | Beginner | Intermediate | Expert | Advanced |
| terrain | Groomed, All-Mountain | Park | Powder, Trees | Backcountry | Groomed |

## Code changes (samu-test)
1. **Add `sections/variant-specs.liquid`** (based on the provided draft, with fixes):
   - Close the JS block with `{% endjavascript %}` (the draft's `{% endschema_placeholder %}` is invalid).
   - Server-render initial values from
     `product.selected_or_first_available_variant.metafields.specs[key]` with `| default: '—'`.
   - Embed `{ "<variantId>": { "<key>": <value>, ... } }` JSON in a `data-vspec-data` script for all variants.
   - **Variant-change hook (Dawn-native, recommended):** subscribe to `PUB_SUB_EVENTS.variantChange`
     from `assets/global.js` to update the cards; keep the draft's `?variant=` URL + form-`change`
     listener as a cross-theme fallback.
   - Apply Figma-derived styles in `{% stylesheet %}`. Settings: `eyebrow`, `heading`,
     `heading_accent`, `subheading`; include `presets` so it appears in the theme editor.
2. **Place the section** — add `variant-specs` to `templates/product.json` `order` (below `main`), or
   add it via the theme editor once the file is saved and `theme dev` is running.

## Files touched
- `sections/variant-specs.liquid` (new)
- `templates/product.json`

## Acceptance criteria
- 8 cards render server-side for the default variant (SEO-safe, no JS required for first paint).
- Switching swatches updates all 8 values to the selected variant's specs.
- Empty metafields fall back to `—`.
- Layout/colors/type match Figma; responsive (4 → 2 → 1 columns).
- No console errors.

## Verification
`http://127.0.0.1:9292/products/the-complete-snowboard`; cycle Ice→Dawn→Powder→Electric→Sunset and
confirm each shows correct values; compare against Figma.
