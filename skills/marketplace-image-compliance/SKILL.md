---
name: marketplace-image-compliance
description: Meet the image rules across marketplaces — dimensions, backgrounds, prohibited elements and AI disclosure — so listings aren't suppressed. Use whenever the user is uploading product images to a marketplace, has had images rejected, asks about image requirements, or is publishing a catalogue across several channels.
---

# Marketplace Image Compliance

Marketplace image rules are enforced automatically, and a rejected main image doesn't just fail — it can suppress the whole listing from search. That makes compliance a revenue issue rather than a formatting one.

The rules differ per marketplace, they change, and **you should verify against the current published spec before a bulk upload.** What follows is the shape of the requirements and where the traps are.

## What almost every marketplace requires

| Rule | Typical requirement |
|---|---|
| Main image background | Pure white, **RGB 255,255,255** |
| Product fill | 85%+ of the frame |
| Main image content | Product only. No props |
| Text, logos, watermarks, badges | **Prohibited on the main image** |
| Borders, frames, inset images | Prohibited |
| Colour space | sRGB |
| Format | JPEG or PNG (TIFF and GIF sometimes) |
| Minimum longest side | 1000px, often 1600px for zoom |
| Aspect ratio | 1:1 for the main image |

**The four that get listings rejected most often:**

1. **Off-white background.** 254,254,254 is not white. It's measured
2. **Text or badges on the main image** — "Best Seller", "50% Off", size charts
3. **Under the minimum dimension**, which also silently disables zoom
4. **Props in the main image**, including packaging the product doesn't ship in

## The traps that aren't obvious

**Accessories in frame.** A main image containing items you don't ship reads as misrepresentation. If the phone case photo includes a phone, the customer expects a phone.

**Packaging as the product.** Showing the box when you sell the contents, or vice versa. Some marketplaces prohibit packaging in the main image entirely.

**Mannequins and hangers.** Several marketplaces prohibit visible mannequins or hangers in apparel main images. Ghost mannequin — where the form is removed and the garment holds its shape — is usually the compliant approach. See `apparel-product-photography`.

**Multiple angles in one image.** A grid of four views in a single file is prohibited on most marketplaces even though it looks useful.

**Colour that doesn't match.** Not always an explicit rule, but it drives returns and returns drive account health metrics. A graded image that shifts the product colour is a commercial problem even where it's technically compliant. See `white-background-ecommerce`.

## AI disclosure

The requirement that's newest and least settled.

Several marketplaces and platforms now require disclosure of AI-generated or AI-modified product imagery, and some prohibit synthetic imagery for the main listing image specifically — on the reasonable basis that the main image is a representation of the actual item.

The defensible position:

- **Use photography for the main listing image** where you can. It's the image the customer relies on
- **Generated lifestyle and secondary images** are widely accepted, and disclose where required
- **Never generate a face of the product you don't have** — a fabricated rear panel or invented label copy is a misrepresentation regardless of disclosure
- **Check the current policy** for each marketplace before a bulk upload. This area is changing quickly

Retouching a real photograph — dust removal, background cleanup, relighting — sits differently from generating an image of the product. The first is normal practice; the second is a representation question. See `inpainting-product-fixes`.

## One master, many outputs

The workflow that keeps a multi-channel catalogue compliant without doing the work five times.

```
1. Real product photography            the source of truth
2. remove_background       2 credits   → clean cutout
3. Composite on 255,255,255 white      → in an editor
4. Add consistent contact shadow       → in an editor
5. Generate/upscale to 4K              → the master
6. Derive every channel output from the master
```

```
4K 1:1 master
  ├─ 2000px 1:1 JPEG sRGB   marketplace main
  ├─ 1600px 1:1             secondary marketplace
  ├─ 1080px 1:1             social
  ├─ 1080×1350 (4:5)        paid social — generate native, don't crop
  └─ 1080×1920 (9:16)       story — generate native
```

**Downscale from a 4K master; never upscale to meet a threshold.** And for ratios that aren't crops of 1:1, generate natively rather than cropping — a 1:1 composition cropped to 9:16 loses the product. See `product-upscaling-4k` and `aspect-ratio-strategy`.

## The pre-upload check

Run this on a sample before a bulk upload, and on every main image.

```
[ ] Background sampled at 5+ points = exactly 255,255,255
[ ] No gradient or vignette in the white
[ ] Product fills 85%+
[ ] No text, logo, watermark, badge or border
[ ] No props, accessories or packaging that don't ship
[ ] No mannequin or hanger visible (apparel)
[ ] Single view only — no grids
[ ] 1:1, sRGB, above the zoom threshold
[ ] Colour matches the real product
[ ] Edges clean — checked on white AND black
[ ] File format accepted by this marketplace
[ ] AI disclosure applied where required
```

**Sample the background at several points.** A gradient reads 255 in the corner and 251 in the middle, and the automated check finds it.

`analyze_image` (1 credit) on a main image will report any text it sees — a fast way to catch a badge you'd stopped noticing.

## Rejections

When an image is rejected:

1. **Read the exact reason.** They're specific, and the fix follows from it
2. **Don't re-upload unchanged.** Repeat rejections affect account standing on some platforms
3. **Fix the master, not the export** — otherwise the fault propagates to every channel
4. **Check the sibling images** for the same fault. It's usually systematic

Systematic faults are the common case: if one image has an off-white background, they all do, because they came from the same process.

## Don't

- **Don't assume the rules are the same across marketplaces.** Check each.
- **Don't ship off-white.** It's measured.
- **Don't put badges or text on the main image.**
- **Don't include accessories you don't ship.**
- **Don't upload a grid of angles as one image.**
- **Don't upscale to reach a threshold.** Generate or photograph larger.
- **Don't use a generated image as the main listing image** where policy prohibits it.
- **Don't fix an export.** Fix the master.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`white-background-ecommerce`, `amazon-listing-images`, `shopify-product-images`, `etsy-listing-photos`, `product-upscaling-4k`, `product-image-qa-review`
