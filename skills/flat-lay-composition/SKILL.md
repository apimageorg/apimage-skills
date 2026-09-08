---
name: flat-lay-composition
description: Generate overhead flat-lay product images with deliberate composition, lighting and prop discipline. Use whenever the user wants a flat lay, an overhead product shot, a top-down arrangement, or styled product imagery for social and secondary listing slots.
---

# Flat-Lay Composition

Flat lay — everything laid out and shot from directly above — is the workhorse format for social and secondary listing images. It's also where generated imagery is unusually reliable, because a top-down view has no perspective to get wrong and no occlusion to invent.

The difficulty is composition. A badly composed flat lay looks like a spilled drawer.

## Why it generates well

| | Angled product shot | Flat lay |
|---|---|---|
| Perspective errors | Common | **None — orthographic** |
| Occlusion invention | Common | Minimal |
| Contact shadow | Must be right | Simple, directly beneath |
| Reflections | Complex | Mostly flat |
| Depth cues | Needed | Not needed |

That makes flat lay the safest generated format for a product that isn't reflective. Fewer things can go wrong.

## The call

```
generate_image(
  model="flux-2-pro",
  reference_images=[PRODUCT_REFS],
  prompt="The same bottle laid on a pale linen surface, shot from "
         "directly overhead, perfectly perpendicular. Arranged with "
         "a sprig of eucalyptus at the upper left and a folded linen "
         "cloth at the lower right, generous negative space in the "
         "upper right. Soft diffused daylight from the left, gentle "
         "shadows falling right. Product unchanged.",
  aspect_ratio="1:1",
  seed=8812
)
```

**"Directly overhead, perfectly perpendicular"** is the clause that matters. Without it the model produces a near-overhead angle with slight perspective, which looks like a mistake rather than a choice.

## Composition rules

**Negative space is the composition.** A flat lay that fills the frame edge to edge is cluttered. Leaving a third of the frame empty is what makes it read as designed — and it gives you somewhere to put text.

```
┌─────────────────────┐
│  prop      ░░░░░░░  │  ░ = negative space, and where text goes
│                     │
│    ██ PRODUCT ██    │  ← the hero, off-centre
│                     │
│  ░░░░░░░      prop  │
└─────────────────────┘
```

**Off-centre the hero.** Dead-centre reads as a catalogue scan. Slightly off-centre with the negative space balanced against it reads as photography.

**Odd numbers of objects.** Three or five props sit better than two or four. This is a genuine compositional rule, not superstition — even groupings pair up and feel static.

**One clear hero.** The product should be unambiguously the subject. Props support; they don't compete. Three items of equal visual weight produces an image with no subject.

**Align to a grid or don't align at all.** Strict alignment reads as deliberate; casual scattering reads as natural. Almost-aligned reads as careless.

## Props

Props are where flat lays go wrong, in two directions.

**Too many.** Every added object dilutes the product. Three props is usually the ceiling for a product shot.

**Wrong relationship.** Props should imply the product's context or use, not just fill space. Eucalyptus beside a skincare bottle says "natural, calm". A random succulent says "stock photo".

Prop vocabulary that works:

| Category | Props |
|---|---|
| Beauty, skincare | Linen, eucalyptus, stone, water droplets, a towel |
| Food | Raw ingredients, a cloth, cutlery, a board |
| Stationery, tech | Notebook, pen, coffee cup, cable, glasses |
| Apparel, accessories | Folded garments, shoes, a bag, jewellery |
| Homeware | Textiles, dried flowers, a ceramic dish |

**The claim risk:** props that look like included accessories. A flat lay of a phone case containing a phone implies the phone. If the props aren't shipped, the copy has to be clear. See `product-image-qa-review`.

## Surfaces and light

| Surface | Reads as |
|---|---|
| Pale linen or cotton | Calm, natural, premium |
| White or grey marble | Clean, premium, cool |
| Pale oak or timber | Warm, domestic, natural |
| Concrete | Modern, industrial |
| Coloured paper | Graphic, brand-forward |
| Textured plaster | Editorial |

**Soft diffused light from one side**, with gentle shadows falling away from it. Named direction, always.

**Avoid overhead-only light** on a flat lay — it produces shadows directly beneath every object, which flattens the whole image and removes the texture that makes a surface read.

## Where flat lay fails

- **Tall products.** A bottle laid on its side may not be how you want it represented, and standing it up defeats the overhead angle
- **Products with a clear "front"** that isn't the top face
- **Reflective products.** The surface reflects in them. See `jewelry-reflective-products`
- **Anything needing depth** to communicate — furniture, bags with structure
- **Very small products alone.** No scale cue. Add a hand or a familiar object. See `product-scale-reference`

## Text overlay

Flat lay is the best product format for text overlay because you can design the negative space for it.

- **Prompt the empty region deliberately**: "generous negative space in the upper right, plain surface, no props"
- **Overlay text in an editor**, never in the generation
- **Keep the text zone low-detail** — text over a busy weave is unreadable at feed size

See `video-caption-subtitle-planning` for the text principles; they apply identically to stills.

## Building a flat-lay set

```python
SURFACE = ("laid on a pale linen surface, shot from directly overhead, "
           "perfectly perpendicular. Soft diffused daylight from the "
           "left, gentle shadows falling right.")

ARRANGEMENTS = [
    "product centred slightly left, eucalyptus sprig upper left, "
    "generous negative space upper right",
    "product lower left, folded cloth upper right, three small stones "
    "in a loose diagonal",
    "product alone, centred slightly high, large negative space below",
]

for arr in ARRANGEMENTS:
    generate_image(model="flux-2-pro", reference_images=REFS,
                   prompt=f"The same product {arr}. {SURFACE} "
                          f"Product unchanged.",
                   aspect_ratio="1:1", seed=8812)
```

Same surface clause verbatim, same seed, same light. Only the arrangement varies. See `product-photo-consistency`.

## Don't

- **Don't omit "perfectly perpendicular."** Near-overhead looks like an error.
- **Don't fill the frame.** Negative space is the composition.
- **Don't centre the hero dead-centre.**
- **Don't use more than three props.**
- **Don't use even numbers** of grouped objects.
- **Don't almost-align.** Align properly or scatter naturally.
- **Don't light from overhead only.** Side light, named direction.
- **Don't include props that read as included accessories** without clarifying.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lifestyle-product-photography`, `multi-product-scene`, `seasonal-product-styling`, `product-scale-reference`, `social-commerce-product-images`, `product-photo-consistency`
