---
name: white-background-ecommerce
description: Produce compliant pure-white product images for marketplace listings, including the shadow and framing rules that get listings rejected. Use whenever the user needs white background product photos, main listing images, Amazon or marketplace hero images, or catalogue images on white.
---

# White-Background Listing Images

The main listing image on almost every marketplace has to be the product on pure white. It's the most rule-bound image in ecommerce and the one most often rejected.

The rules exist because the main image appears as a thumbnail in a grid next to competitors, and consistency is what makes that grid readable.

## The spec most marketplaces converge on

| Requirement | Typical rule |
|---|---|
| Background | **Pure white — RGB 255,255,255** |
| Product fill | **85% or more of the frame** |
| Additional props | **None.** Product only |
| Text, logos, watermarks, badges | **None** |
| Borders and frames | None |
| Shadow | Usually permitted if soft and natural |
| Format | JPEG or PNG, sRGB |
| Aspect ratio | 1:1 for the main image |
| Minimum dimension | Commonly 1000px+, often 1600px+ for zoom |

**"Pure white" means exactly 255,255,255**, not off-white, not #FEFEFE. Automated checks measure it. A background that looks white and reads as 253 gets flagged.

Always verify against the specific marketplace's current spec — they differ and they change. See `marketplace-image-compliance`.

## The workflow

```
1. remove_background     2 credits   → clean transparent cutout
2. Composite on 255,255,255 white    → in an editor
3. Add a soft contact shadow          → in an editor, if permitted
4. Frame to 85%+ fill, 1:1
5. Export sRGB, JPEG or PNG
```

**Cut out, then composite. Don't generate a white background.** Two reasons:

- Generated "white" is rarely exactly 255,255,255 — it has gradients, subtle tints and vignetting
- You need control over the shadow, and a generated shadow isn't consistent across a catalogue

The cutout is the reliable path and it's 2 credits. See `background-removal-workflow`.

## The shadow question

Marketplaces generally permit a natural shadow and prohibit a stylised one. In practice:

**Usually fine:** a soft contact shadow directly beneath the product, low opacity, short.

**Usually rejected:** long dramatic shadows, coloured shadows, reflections, gradient "floor" effects, drop shadows offset like a UI element.

**Keep it identical across the catalogue.** Same offset, same blur, same opacity. A grid where every product has a slightly different shadow looks amateur even when each image passes.

Some sellers omit the shadow entirely for a floating look. That's compliant and it's a defensible choice — just be consistent.

## Framing and fill

The 85% rule is enforced and it's the second most common rejection after background colour.

```
┌─────────────────────┐
│  ~7% margin         │
│  ┌───────────────┐  │
│  │               │  │
│  │    PRODUCT    │  │  ← 85%+ of the frame
│  │               │  │
│  └───────────────┘  │
│  ~7% margin         │
└─────────────────────┘
```

- **Fill it.** A product floating small in a white square wastes the thumbnail and fails the rule
- **Small, even margins.** Not touching the edge, not swimming
- **Consistent across the catalogue.** Same relative product size for products of similar shape
- **Orient consistently.** All bottles upright, all boxes at the same angle

That last one matters more than it sounds. A category page where every product sits at a different angle is visually noisy in a way customers feel and can't name.

## Colour accuracy is the commercial risk

This is where white-background images cause real problems rather than rejections.

A graded lifestyle render shifts colour. If your listing image shows a navy shirt as charcoal, the customer receives something that doesn't match and returns it.

- **Neutral white balance, always**, for the main image
- **Check against the real product**, not against a graded reference photo
- **State it in the prompt** if generating any part of it: "neutral white balance, accurate colour, no grade"
- **Keep graded versions for lifestyle images**, which sit further down the listing

Returns driven by colour mismatch are expensive and they're attributable to the image. See `product-image-qa-review`.

## Resolution

Generate large. Marketplace zoom features need it, and a 4K master crops into every other required ratio without softening.

| Purpose | Resolution |
|---|---|
| Main listing image | Full HD minimum, **4K preferred** |
| Zoom-enabled listings | 4K |
| Category thumbnails | Derived from the master |

Upscale an under-sized source with `edit_image` rather than shipping a soft image. See `product-upscaling-4k`.

## Generating additional angles

The main image needs the front. The listing also wants back, side, detail and in-use — and those can come from the same reference set.

```
generate_image(
  model="flux-2-pro",
  reference_images=[FRONT, SIDE, BACK, DETAIL],
  prompt="The same product, three-quarter view from the left, on "
         "pure white, soft even studio light, neutral white balance, "
         "accurate colour. Product unchanged.",
  aspect_ratio="1:1",
  seed=8812
)
```

Then cut out and composite each onto true white, same as the main image. **Supply references covering the angles you're asking for** — a front-only reference asked for a rear view produces an invented back. See `camera-angle-variation`.

## The QA pass

```
[ ] Background sampled at several points = exactly 255,255,255
[ ] No vignetting or gradient in the white
[ ] Product fills 85%+
[ ] No props, text, logos, watermarks or badges
[ ] Shadow soft and consistent with the rest of the catalogue
[ ] Colour matches the real product
[ ] 1:1, sRGB, at or above the minimum dimension
[ ] Edges clean — no halo, checked against black too
[ ] Consistent orientation and scale with sibling products
```

**Sample the background at several points, not one.** A gradient reads as white in the corner and 251 in the middle.

## Don't

- **Don't generate the white background.** Cut out and composite on exact 255,255,255.
- **Don't ship off-white.** It's measured.
- **Don't add props, text or badges** to the main image.
- **Don't use a long or stylised shadow.**
- **Don't vary the shadow across the catalogue.**
- **Don't use a graded render** for the main image. Neutral.
- **Don't leave the product small** in the frame.
- **Don't sample the background at one point.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-removal-workflow`, `marketplace-image-compliance`, `amazon-listing-images`, `product-upscaling-4k`, `camera-angle-variation`, `product-image-qa-review`
