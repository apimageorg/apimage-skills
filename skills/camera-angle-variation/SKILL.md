---
name: camera-angle-variation
description: Generate a product from new angles using edit_image camera adjustment or reference sets, and know which angles are recoverable. Use whenever the user needs more angles of a product, wants a three-quarter or top-down or side view, or has only one product photo and needs a full listing set.
---

# Camera Angle Variation

Listings want front, three-quarter, side, back, top and detail. Most sellers have one photo. Generating the rest is the highest-value product photo workflow there is — and the one with the clearest failure mode.

**Any angle that reveals a surface your references don't show will be invented.** The model doesn't know what the back looks like, so it makes one up, and it will be wrong.

## Two routes

**`edit_image` camera angle adjustment** — modest reframing of the same shot. Good for small shifts, cheaper in composition terms because it preserves the scene.

**Reference-based generation** — supply reference images covering the product, then generate a new view. Necessary for anything beyond a small shift.

```
generate_image(
  model="flux-2-pro",
  reference_images=[FRONT, SIDE, BACK, TOP],
  prompt="The same product, three-quarter view from the upper left, "
         "on pure white, soft even studio light, neutral white "
         "balance. Product unchanged — same label, shape and colour.",
  aspect_ratio="1:1",
  seed=8812
)
```

`reference_images` accepts **up to 4 for images**. For angle work, use all four and choose them to cover the faces.

## What's recoverable from what

| You have | Reliably generate | Risky | Invented |
|---|---|---|---|
| Front only | Slight rotation, tighter/wider crop | Three-quarter | Side, back, top |
| Front + back | Both, plus slight rotations | Three-quarter | True side, top |
| Front + side | Three-quarter between them | Rear three-quarter | Back, top |
| Front + side + back | Most three-quarters, near-side views | Top-down | Underside |
| Four faces | Almost any angle | Extreme perspective | — |

**The rule: interpolating between references is reliable; extrapolating beyond them is invention.**

A three-quarter view sits between a front and a side, so with both references it's genuinely derivable. A back view from a front-only reference is fabrication.

## Prioritise which references to capture

If you can get more photographs of the real product, get these four:

1. **Front, straight on** — the main listing image and the primary reference
2. **Side, straight on** — unlocks all the three-quarters
3. **Back** — needed for any rear view, and often has required regulatory copy
4. **Top-down** — unlocks overhead and flat-lay work

Those four unlock nearly every angle a listing needs. Anything less and the gaps get filled by the model.

## Angle vocabulary that works

```
straight on / front elevation
three-quarter view from the left / right
side profile, straight on
rear three-quarter
top-down / overhead / flat lay
low angle, looking up
raised angle, looking slightly down
45 degrees above, 45 degrees to the left
eye-level
hero angle — slightly above and to the side
```

**"Three-quarter view from the upper left"** is the workhorse angle for product listings — it shows the front and one side simultaneously, which reads as three-dimensional. Most catalogues under-use it.

Also state the **camera height** as well as the rotation. "Three-quarter view" alone leaves the vertical ambiguous, and the model picks.

## Keep the set consistent

A listing set with each image at a different distance, height and light looks unprofessional even when each image is individually fine.

```python
BASE = ("On pure white, soft even studio light from above and slightly "
        "left, neutral white balance, accurate colour, subtle contact "
        "shadow. Product fills 85% of frame. Product unchanged.")

ANGLES = [
    "straight on, front elevation",
    "three-quarter view from the upper left",
    "side profile, straight on, from the left",
    "rear three-quarter from the upper right",
    "top-down, directly overhead",
]

for angle in ANGLES:
    generate_image(
        model="flux-2-pro",
        reference_images=[FRONT, SIDE, BACK, TOP],
        prompt=f"The same product, {angle}. {BASE}",
        aspect_ratio="1:1",
        seed=8812,
    )
```

**Same seed, same base clause verbatim, same references, same ratio.** Only the angle varies. That's what produces a set rather than a collection. See `product-photo-consistency`.

## Check what the model invented

The QA pass specific to angle work.

```
[ ] Compare each generated angle against the real product
[ ] On any face your references didn't cover — is it fabricated?
[ ] Label text: correct, or invented?
[ ] Proportions consistent across angles?
[ ] Details that should appear on the side — do they?
[ ] Details that shouldn't exist — did the model add them?
[ ] Same colour across every angle?
```

**Fabricated rear panels are the classic error.** The model generates a plausible-looking back with invented text, invented ports, invented markings. It looks fine and it's a false representation of the product.

If you can't verify a face against the real product, don't publish that angle. Photograph it instead.

`analyze_image` (1 credit) on a generated angle will read out what it sees, including invented text — a useful cross-check.

## Regulatory faces need photographs

Worth stating separately. Any face carrying required information — ingredients, warnings, certifications, nutrition, safety marks — must be a **photograph, not a generation**. Models render text badly and inventing regulatory copy is a compliance problem, not an aesthetic one.

Generate the marketing angles. Photograph the regulatory ones.

## Cost

| Approach | Cost |
|---|---|
| `edit_image` angle adjustment | generation credits, preserves scene |
| `generate_image` with references | 1-9 credits per angle |
| A full five-angle set | roughly 5-45 credits total |

Cheap relative to a photoshoot. Iterate at low resolution, finalise at Full HD or 4K. See `image-model-selection`.

## Don't

- **Don't generate an angle your references don't cover.**
- **Don't publish a fabricated rear panel.**
- **Don't generate any face carrying regulatory text.** Photograph it.
- **Don't vary distance, height or light** across a listing set.
- **Don't omit camera height** from the prompt. "Three-quarter" is ambiguous.
- **Don't skip the real-product comparison.**
- **Don't use one reference** when the model accepts four.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-from-reference`, `white-background-ecommerce`, `product-photo-consistency`, `product-detail-macro`, `product-image-qa-review`, `marketplace-image-compliance`
