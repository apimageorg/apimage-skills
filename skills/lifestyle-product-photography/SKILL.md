---
name: lifestyle-product-photography
description: Generate product images in real-world context — in use, in a setting, with people — rather than isolated on white. Use whenever the user wants lifestyle shots, in-context product photos, in-situ imagery, or images for ads and social rather than listings.
---

# Lifestyle Product Photography

White-background images tell a customer what the product *is*. Lifestyle images tell them what having it *is like*, and that's what sells on social, in ads and further down a listing.

They're also where generated imagery is at its most useful, because context is exactly what's expensive to photograph and cheap to generate.

## Lifestyle vs listing images

| | Listing image | Lifestyle image |
|---|---|---|
| Background | Pure white, mandated | A real setting |
| Job | Identification, comparison | Desire, context, scale |
| Grade | **Neutral, accurate** | Graded for mood |
| Props | None allowed | Encouraged |
| People | Usually not | Often |
| Where it runs | Main listing slot | Ads, social, secondary slots |
| Accuracy standard | Strict | Still strict on the product |

**Keep the two separate.** A graded lifestyle render is the wrong image for the main listing slot — the grade shifts the product's colour and the customer receives something that doesn't match. See `white-background-ecommerce`.

## The two routes

**`replace_background`** — 3 credits, swaps the scene and relights the subject. Fast, reliable, and the right default when you have a clean product shot.

```
replace_background(
  image=PRODUCT_CUTOUT,
  prompt="Pale oak kitchen counter, soft morning light from a window "
         "on the left, blurred kitchen behind at f/2.8, warm neutral "
         "grade, subtle contact shadow, a folded linen cloth just "
         "out of focus in the foreground"
)
```

**`generate_image` with references** — more control over composition, props and hands, at 1-9 credits.

```
generate_image(
  model="flux-2-pro",
  reference_images=[PRODUCT_REFS],
  prompt="The same bottle standing on a pale oak counter, hands "
         "reaching for it from the right, blurred kitchen behind. "
         "Soft morning window light from the left, warm neutral "
         "grade, shallow depth of field. Product unchanged.",
  aspect_ratio="4:3",
  seed=8812
)
```

Use `replace_background` for a scene swap. Use `generate_image` when the composition itself needs to change — props, hands, people, framing.

## What makes a lifestyle shot work

**Specificity of setting.** "Kitchen" produces a generic kitchen. "Pale oak counter, morning light, a folded linen cloth, blurred kettle behind" produces a place.

**A hint of human presence.** Not necessarily a person — a hand, a used cup, a book left open, a slightly rumpled cloth. The signal that someone lives here.

**Believable imperfection.** A perfectly styled scene reads as a catalogue. A cloth slightly askew, a crumb, uneven light reads as real. This is the same principle as prompting against polish in social video.

**Depth.** Shallow focus with a blurred background separates the product and makes the scene feel photographed rather than composited.

**Light with a direction.** Flat even light reads as studio. Directional light with falloff reads as a room.

```
Weak:   "product in a modern kitchen, professional photo, high quality"
Strong: "Bottle on a pale oak counter beside a folded linen cloth,
         morning light raking in from a window on the left, blurred
         kettle and open shelf behind at f/2.0, warm neutral grade,
         a faint water ring on the wood"
```

That water ring is doing real work. It's the detail that makes it a place rather than a set.

## Building a scene library

Define scenes once, apply to every product. Save the good ones.

```python
SCENES = {
    "kitchen_morning": ("Pale oak counter, soft morning light raking "
        "from a window on the left, blurred kitchen behind at f/2.8, "
        "folded linen cloth in soft foreground, warm neutral grade."),
    "bathroom_bright": ("White marble surface, bright even daylight, "
        "blurred tiled wall behind, a small rolled towel, cool "
        "neutral grade, clean and calm."),
    "desk_focused": ("Matte dark wood desk, soft overhead light with "
        "gentle falloff, blurred notebook and pen, neutral grade."),
    "outdoor_table": ("Weathered timber table, dappled afternoon sun "
        "through leaves, blurred foliage behind, warm grade."),
}

create_brand_asset(type="background", ...)     # free — save the keepers
```

A scene library plus a product library means a new product gets a full lifestyle set for a handful of credits, in the same visual language as the rest of the catalogue. See `product-photo-consistency`.

## Product accuracy still applies

The relaxation is in the background, not the product.

- **The product must still be the product** — same shape, label, colour, proportions
- **Reference-based, always.** Don't let a lifestyle prompt reinvent the product
- **Check colour after grading.** A warm scene makes navy read as charcoal
- **Don't show a configuration you don't sell**
- **Don't imply an accessory is included** if it isn't. A styled scene containing items you don't ship is a claim problem

That last one is the lifestyle-specific trap. A beautifully styled scene with three complementary items reads as "these come together" unless the copy is clear. See `product-image-qa-review`.

## People in lifestyle shots

If a person appears, the rights questions from video apply identically:

- **Don't use a real person's likeness** without permission — including stock portraits whose licence excludes synthetic use
- **Generate a consistent model** and save them as a character brand asset
- **Don't imply endorsement** by a real or apparently-real individual
- **Hands only** is often the better choice — human presence without the identity or the uncanny-face problem

Hands are hard to generate, so keep them partially cropped, in motion, and not the focal point. See `product-in-hand-shots` and `ai-avatar-presenter`.

## Where lifestyle images run

| Placement | Ratio | Notes |
|---|---|---|
| Secondary listing slots | 1:1 | After the white main image |
| Instagram feed | 1:1 or 3:4 | 3:4 gets more feed height |
| Story / Reels still | 9:16 | Leave the middle third clear for text |
| Paid social | 1:1 and 4:5 | Generate both natively |
| Email and web | 16:9 or 4:3 | |
| Pinterest | 3:4 or 9:16 | Tall performs |

Generate at the target ratio rather than cropping — a lifestyle composition framed for 1:1 doesn't survive a crop to 9:16. See `aspect-ratio-strategy`.

## Don't

- **Don't use a lifestyle render as the main listing image.**
- **Don't say "modern kitchen."** Name the surface, the light and one detail.
- **Don't style it perfectly.** Imperfection reads as real.
- **Don't use flat even light** for a lifestyle shot.
- **Don't let the prompt reinvent the product.** Use references.
- **Don't include items you don't sell** without making that clear.
- **Don't use a real person's likeness** without permission.
- **Don't crop between ratios.** Generate native.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-replacement-scenes`, `product-in-hand-shots`, `flat-lay-composition`, `seasonal-product-styling`, `product-photo-consistency`, `social-commerce-product-images`, `ugc-lifestyle-photos`
