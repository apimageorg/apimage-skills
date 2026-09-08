---
name: shopify-product-images
description: Build product imagery for a Shopify store — variant images, collection consistency, theme aspect ratios and performance. Use whenever the user runs a Shopify store, is preparing product images for their own site, or asks about variant images or collection page consistency.
---

# Shopify Product Images

Your own store is the opposite constraint from a marketplace: no compliance rules, complete freedom, and therefore no external forcing function for consistency. Most stores end up with a collection page that looks assembled rather than designed.

The three things that actually matter on Shopify: **collection page consistency, variant images, and file weight.**

## Collection page consistency is the whole game

The collection page is a grid, and it's judged as one. A grid where every product has a different background tone, scale, angle and shadow reads as unprofessional even when each image is individually fine — and it's the page most of your traffic lands on.

```
Fixed across every product:
  aspect ratio            1:1 (or whatever the theme uses)
  background              identical, exactly
  product fill            same % for similar shapes
  primary angle           same convention
  light direction         same
  shadow                  same offset, blur, opacity
  grade                   neutral, no per-product variation
```

**Lay the collection out as a grid and look at it.** That's the highest-value QA step for a Shopify store and it takes minutes. See `product-photo-consistency`.

## Aspect ratio: check the theme

Shopify themes crop product images differently, and a mismatch produces cropped products on the collection page.

| Theme behaviour | What to supply |
|---|---|
| Square crop | **1:1** |
| Portrait crop (4:5, 2:3) | 3:4, or the theme's exact ratio |
| Natural / no crop | Any, but be consistent |
| Fills a container | Generate the container's ratio |

**Find out what the theme does before generating the catalogue.** Supplying 1:1 to a theme that crops to 4:5 removes the top and bottom of every product.

Most themes centre-crop, so keeping the product centred with margin protects it either way. Generate at the theme's ratio where you can. See `aspect-ratio-strategy`.

## Variant images

Shopify's variant image feature is where stores most often fall short, and it directly affects conversion and returns.

**Every purchasable variant should have its own image.** A colour selector that doesn't change the photo makes the customer guess, and guessing produces returns.

```
For each colour variant:
  1. Photograph or reference the REAL variant
  2. Same angle, same light, same fill, same background as siblings
  3. Assign to the variant in Shopify
```

**Never recolour a product to create a variant image.** It's the tempting shortcut and it's the wrong one:

- The generated colour won't match the real dye or finish
- You may generate a colourway you don't actually stock
- Colour mismatch is the single largest driver of apparel and homeware returns

Photograph or reference each real variant. If a variant genuinely can't be photographed, don't publish a generated approximation — publish a swatch and say so in the copy. See `product-image-qa-review`.

## The product page gallery

No marketplace rules, so the sequence should be built for how people actually scroll.

| Position | Image |
|---|---|
| 1 | Hero — clean, consistent, the collection page thumbnail |
| 2 | Scale or in-use — answers "how big" immediately |
| 3 | Detail / material — the quality signal |
| 4 | Lifestyle in context |
| 5 | What's included, if a set |
| 6 | Alternative angle or back |
| 7 | Size or comparison chart, if relevant |

**Position 2 should not be another angle.** Scale and use are what a customer wants second, and most stores put a rear view there.

Generate positions 3, 4 and 6 from a cutout with `replace_background` and `product-detail-macro` at a few credits each.

## File weight matters here

Unlike a marketplace, you own the page speed — and product images are usually the heaviest thing on it.

- **Serve WebP.** Considerably smaller than PNG or JPEG at equivalent quality, and Shopify handles conversion
- **Don't upload 4K to display at 800px.** Keep the 4K master, upload a sensible size
- **Use transparent PNG only when you need transparency**, not by default
- **Consistent dimensions** across the catalogue, so the theme isn't resizing unpredictably

`remove_background` outputs transparent PNG **or WebP** — use WebP for web delivery. See `background-removal-workflow`.

A collection page loading twenty 2MB images is slow, and slow collection pages lose sales in a way no image quality recovers.

## Building the catalogue

```python
LOOK = ("On a consistent pale neutral background, soft even light "
        "from above and slightly left, neutral white balance, "
        "accurate colour, subtle contact shadow. Product centred, "
        "filling 80% of frame with even margin. Product unchanged.")

for product in catalogue:
    cutout = remove_background(image=product["photo"])     # 2 credits
    create_brand_asset(type="product", ...)                # free
    hero = generate_image(model="flux-2-pro",
                          reference_images=[cutout],
                          prompt=f"The same product, straight on. {LOOK}",
                          aspect_ratio=THEME_RATIO,
                          seed=8812)
```

**80% fill with even margin** rather than a marketplace's 85% — your theme adds its own padding, and a product touching the edge of the frame looks cramped in a card.

See `product-photo-batch-pipeline`.

## Where a store differs from a marketplace

Worth being explicit, because the habits transfer badly.

| | Marketplace | Your Shopify store |
|---|---|---|
| Background | Mandated pure white | **Your choice — be consistent** |
| Text on images | Prohibited on main | Allowed, use sparingly |
| Props | Prohibited on main | Allowed |
| Brand expression | Minimal | **The point** |
| Consistency enforcement | External | **Yours to maintain** |
| Page speed | Their problem | **Yours** |

**A consistent off-white or pale tone often looks better than pure white on a store**, because it separates the product from the page background. Just pick one and hold it across the whole catalogue.

## The QA pass

```
[ ] Collection grid review at thumbnail size
[ ] Same background tone across every product
[ ] Same fill and margin for similar shapes
[ ] Theme crop tested — nothing important cut
[ ] Every purchasable variant has its own real image
[ ] No recoloured variant images
[ ] Position 2 is scale or use, not another angle
[ ] File sizes sensible, WebP where possible
[ ] Colour verified against physical products
```

## Don't

- **Don't recolour products for variant images.**
- **Don't leave variants without images.**
- **Don't generate the catalogue before checking the theme's crop.**
- **Don't put another angle in position 2.** Scale or use.
- **Don't upload 4K for an 800px display.**
- **Don't vary the background tone** across the catalogue.
- **Don't judge images individually.** Grid review.
- **Don't ignore page weight.** Slow collection pages lose sales.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-consistency`, `background-removal-workflow`, `product-photo-batch-pipeline`, `amazon-listing-images`, `etsy-listing-photos`, `product-image-qa-review`
