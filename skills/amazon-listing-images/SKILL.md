---
name: amazon-listing-images
description: Build a complete Amazon image set — compliant main image, the six-plus gallery slots, and what each slot should do. Use whenever the user is preparing Amazon listing images, A+ content, or asks about Amazon image requirements and gallery strategy.
---

# Amazon Listing Images

Amazon has the strictest main-image rules of any major marketplace and the most structured gallery. Getting the main image wrong suppresses the listing; getting the gallery wrong just loses conversions quietly.

**Verify against Amazon's current published requirements before uploading** — they change, they differ by category, and what follows is the shape rather than the spec.

## The main image

| Requirement | Rule |
|---|---|
| Background | **Pure white, RGB 255,255,255** |
| Content | Product only, as sold |
| Fill | **85%+ of the frame** |
| Text, logos, watermarks, badges | **Prohibited** |
| Borders, insets, colour blocks | Prohibited |
| Props and accessories not included | Prohibited |
| Mannequins (apparel) | Prohibited visible |
| Format | JPEG, TIFF, PNG or GIF; sRGB |
| Longest side | 1000px minimum, **1600px+ enables zoom** |
| Shape | Square strongly preferred |

**Zoom is the one to prioritise.** Above 1600px on the longest side Amazon enables its zoom viewer, and listings with zoom convert better. Shipping at 1200px is compliant and quietly leaves conversion on the table.

The workflow:

```
1. Photograph the product
2. remove_background              2 credits
3. Composite on exact 255,255,255
4. Soft contact shadow, consistent across the catalogue
5. Frame to 85%+ fill, square
6. Export sRGB at 2000px+
```

See `white-background-ecommerce` and `marketplace-image-compliance`.

## The gallery, slot by slot

Amazon allows several additional images, and each slot has a job. Most sellers fill them with more angles, which wastes them.

| Slot | Job |
|---|---|
| **1 — Main** | Compliant white background. Identification |
| **2 — Scale / in hand** | How big is it. Cuts returns |
| **3 — In use / in context** | What having it is like |
| **4 — Key feature close-up** | The differentiator, with a callout |
| **5 — What's included** | Everything in the box, counted |
| **6 — Comparison or spec** | Sizes, variants, dimensions |
| **7 — Lifestyle / aspirational** | Desire |
| Video | Demonstration. See `product-demo-video` |

**Slots 2 and 5 are the return-reducers** and they're the most commonly missing. Size confusion and "I thought X was included" are the two biggest image-driven return causes.

**Text is allowed in gallery images** (unlike the main image), which is why slots 4, 5 and 6 work — a callout, a contents list, a dimension overlay. Overlay it in an editor, never generate it.

## Generating the gallery

```
1. Main         photograph → cutout → white composite
2. Scale        photograph in hand, or generate carefully. See product-in-hand-shots
3. In use       replace_background         3 credits per scene
4. Feature      product-detail-macro + overlaid callout
5. Included     multi-product-scene, composited, counted
6. Comparison   composited at true relative scale, dimension overlay
7. Lifestyle    lifestyle-product-photography
```

`replace_background` at 3 credits per scene makes slots 3 and 7 nearly free once you have a cutout. That's the argument for doing this properly rather than filling slots with angles.

## A+ content

A+ (enhanced brand content) modules use different dimensions and permit brand imagery, text and comparison tables.

- **Wider, banner-shaped images** — 16:9 and wider. Generate native, don't crop from square
- **Text is permitted**, so overlay copy in the editor
- **Comparison tables** need consistent product images across the row — same angle, same fill, same light. See `product-photo-consistency`
- **Brand story modules** want lifestyle imagery, not white backgrounds
- **Keep a 4K master** and derive every module size from it. See `product-upscaling-4k`

## The accuracy rules that matter on Amazon

Amazon enforces representation, and the consequences run from image rejection to listing suppression to account health impact.

- **The main image must show the product as sold** — not a bundle, not with accessories, not the packaging if you sell the contents
- **Nothing in any image that isn't included** without a clear "not included" label
- **Colour must match.** Colour-driven returns affect your account metrics, not just your margin
- **Don't generate a product face you can't verify.** A fabricated rear panel is a misrepresentation
- **Regulatory faces get photographed** — ingredients, warnings, certifications, nutrition

**On AI-generated imagery:** policy in this area is moving. The defensible position is photography for the main image (it's the representation the customer relies on) and generated imagery for lifestyle and scene variants, disclosed where required. Check the current policy before a bulk upload. See `marketplace-image-compliance`.

## The pre-upload check

```
MAIN IMAGE
[ ] Background sampled at 5+ points = exactly 255,255,255
[ ] No gradient or vignette
[ ] 85%+ fill, square
[ ] No text, logo, badge, border or watermark
[ ] Product only, as sold, no extra accessories
[ ] No visible mannequin or hanger
[ ] 1600px+ longest side for zoom
[ ] sRGB, accepted format
[ ] Colour matches the physical product

GALLERY
[ ] Scale image present
[ ] What's-included image present, count correct
[ ] Any overlaid text legible at thumbnail size
[ ] Nothing implied as included that isn't
[ ] Consistent look across the set — grid review
```

**Grid review the whole set at thumbnail size.** That's how it appears in the gallery strip, and inconsistency shows there first. See `product-image-qa-review`.

## Don't

- **Don't ship off-white.** It's measured.
- **Don't put text or badges on the main image.**
- **Don't ship under 1600px.** It disables zoom.
- **Don't fill the gallery with more angles.** Each slot has a job.
- **Don't skip the scale and what's-included slots.** They cut returns.
- **Don't include accessories you don't ship.**
- **Don't crop A+ banners from a square master.** Generate native.
- **Don't generate regulatory faces.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`white-background-ecommerce`, `marketplace-image-compliance`, `product-scale-reference`, `multi-product-scene`, `shopify-product-images`, `product-image-qa-review`
