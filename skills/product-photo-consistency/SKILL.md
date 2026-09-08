---
name: product-photo-consistency
description: Keep a whole catalogue of product images visually consistent using brand assets, verbatim look clauses and locked seeds. Use whenever the user has many products to shoot, a catalogue that looks inconsistent, or wants images that read as one brand rather than many attempts.
---

# Catalogue Consistency

A category page is judged as a grid, not as individual images. Forty technically-good photos with forty different lighting setups, distances and grades looks amateur — and customers read that as a signal about the products.

Consistency is cheap to achieve and it's mostly a matter of deciding things once and then not re-deciding them.

## The four things to fix

| Element | How | Cost |
|---|---|---|
| **Product references** | `create_brand_asset(type="product")` | free |
| **Backgrounds and scenes** | `create_brand_asset(type="background")` | free |
| **The look** — light, grade, lens | `create_brand_asset(type="preset")` | free |
| **The rendering character** | A locked `seed` per family | free |

Every one of them is free. The only cost is the discipline of saving things at the moment you approve them rather than reconstructing them later.

## Seeds give a look, assets give a subject

The distinction people get wrong, and it matters here as much as in video.

```
seed=8812  "product A on marble, soft light"   → consistent treatment
seed=8812  "product B on marble, soft light"   → same treatment, product B
```

A locked seed produces a consistent *visual treatment* — grade, light quality, rendering character. It does **not** guarantee the product is accurate. That's what `reference_images` and product brand assets do.

You want both. See `seed-locked-iteration` and `product-photo-from-reference`.

## Write the look clause once, copy it verbatim

The single highest-leverage habit. Define the look as a literal string and paste it into every prompt.

```python
LOOK = ("On pure white, soft even studio light from above and slightly "
        "left, gentle falloff to the lower right, neutral white "
        "balance, accurate colour, subtle contact shadow beneath. "
        "Product fills 85% of frame.")

for product in catalogue:
    generate_image(
        model="flux-2-pro",
        reference_images=product["refs"],
        prompt=f"The same product, straight on, front elevation. {LOOK} "
               f"Product unchanged.",
        aspect_ratio="1:1",
        seed=8812,
    )
```

**Paraphrasing is the failure.** "Soft light from the upper left" and "gentle light from above-left" are the same sentence to a human and different prompts to a model. Copy-paste, never rewrite.

## The catalogue spec

Decide these once and write them down. Then every image conforms.

```
Aspect ratio      1:1 for listings
Resolution        Full HD minimum, 4K for masters
Background        pure white 255,255,255 for listings
Light             soft even studio, upper left, 45 degrees
Grade             neutral white balance, no creative grade
Shadow            soft contact shadow, same offset and opacity
Product fill      85% of frame
Primary angle     straight on, front elevation
Secondary angles  three-quarter upper left, side left, rear three-quarter, top
Orientation       all bottles upright, all boxes at the same angle
Seed              8812 for the white-background family
```

The two most-neglected lines are **orientation** and **product fill**. A grid where every product sits at a slightly different angle and a slightly different size is visually noisy in a way customers feel without being able to name.

## Consistency across a product line

For a line with several variants — colours, sizes, flavours — the requirement is stricter, because those images sit directly next to each other.

```
Same seed. Same look clause. Same angle. Same fill. Same shadow.
Only the product changes.
```

The customer is comparing variants, so any difference in treatment reads as a difference in the product. A navy variant photographed slightly warmer than the black one looks like a different material.

**Never recolour a product to create a variant image.** Photograph or reference the real variant. A generated colour is a colour you may not actually sell, and it drives returns.

## Handling supplied photos that don't match

The common real problem: fifty images from three sources with three different looks.

```
1. Pick the best-matching existing image as the reference standard
2. analyze_image on it (1 credit) → get its lighting described
3. Use that description as the look clause
4. For each odd image:
     remove_background   2 credits    → isolate the product
     re-composite / replace_background → the standard scene
     edit_image relight  → the standard look
5. Compare all against the standard, side by side
```

Using `analyze_image` on your own good image to extract a lighting description is a genuinely useful trick — it produces the description in the right vocabulary. See `product-relighting`.

## Review the grid, not the image

The QA step that matters and gets skipped.

**Lay the whole set out as a grid at thumbnail size and look at it.** Individually-fine images reveal their inconsistency immediately in a grid — one is warmer, one is bigger, one has a harder shadow, one is at a different angle.

That grid is exactly how a customer sees a category page. Judge it that way.

```
[ ] Same background tone across every image
[ ] Same product scale for similar-shaped products
[ ] Same shadow character
[ ] Same colour temperature
[ ] Same orientation convention
[ ] Same crop margins
[ ] Nothing that draws the eye for the wrong reason
```

## Maintaining it over time

Consistency degrades as products get added by different people months apart.

- **Write the spec down** somewhere the next person will find it
- **Save the presets as brand assets** — they're the machine-readable version of the spec
- **Keep the seed recorded** with the spec
- **Re-review the grid** whenever a batch is added, not just at launch
- **Version the spec** if the look changes, and decide whether to reshoot the back catalogue

Changing the catalogue look is a real project. Better to choose a durable one than a fashionable one.

## Don't

- **Don't paraphrase the look clause.** Copy it verbatim.
- **Don't rely on seeds for product accuracy.** Use references.
- **Don't recolour a product** to make a variant image.
- **Don't vary orientation** across a category.
- **Don't judge images individually.** Judge the grid.
- **Don't skip the brand assets.** They're free and they are the spec.
- **Don't let a new batch be added** without a grid review.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-from-reference`, `camera-angle-variation`, `product-relighting`, `white-background-ecommerce`, `product-photo-batch-pipeline`, `product-image-qa-review`
