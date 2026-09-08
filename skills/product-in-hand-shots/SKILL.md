---
name: product-in-hand-shots
description: Generate products held in hands, which communicates scale and use — and handle the fact that hands are what generative models get wrong most. Use whenever the user wants a product held, an in-use shot, hands in frame, or a scale reference via a hand.
---

# Products in Hand

A hand holding a product does three jobs at once: it establishes scale, it demonstrates use, and it adds human presence. That's why it's one of the highest-converting gallery images.

It's also the shot most likely to produce a six-fingered hand.

## Why hands are hard

Hands have many joints, they self-occlude, they deform, and the model has seen them in every configuration. The result is the well-known failure mode: extra fingers, missing fingers, joints bending the wrong way, thumbs on the wrong side, fingers merging into the product.

None of it is fixable by prompting harder. It's mitigated by **giving the model less hand to get wrong.**

## The mitigations, in order of effectiveness

**1. Crop the hand.** A hand entering frame from the edge, cropped at the wrist or mid-forearm, has far less visible structure than a full hand on a plain background.

**2. Wrap the fingers.** Fingers curled around the product hide most of the joints. Splayed fingers expose every one of them.

**3. Occlude with the product.** A hand mostly behind the product shows only what's needed.

**4. Keep it out of focus.** A hand at the edge of a shallow depth of field is softer and more forgiving.

**5. One hand, not two.** Two hands doubles the failure surface and adds the problem of them relating correctly.

**6. Just photograph it.** For a critical gallery image, one real photo of a hand holding the product removes the entire risk class. Then generate scenes around it with `replace_background`.

That last option is the honest recommendation for anything going on a listing.

## The call

```
generate_image(
  model="flux-2-pro",
  reference_images=[PRODUCT_REFS],
  prompt="The same jar held in one adult hand, fingers wrapped "
         "around it naturally, thumb visible on the near side. Hand "
         "cropped at the wrist, entering from the lower right. Plain "
         "neutral background, soft even light. Shallow depth of "
         "field with the hand slightly softer than the product. "
         "Product unchanged. Hand at natural adult scale.",
  aspect_ratio="1:1",
  seed=8812
)
```

Two clauses carry unusual weight:

**"Hand at natural adult scale."** Without it the model scales the hand to suit the composition, which makes the product look the wrong size — and since one purpose of this shot is communicating scale, that's actively misleading. See `product-scale-reference`.

**"Fingers wrapped around it naturally, thumb visible on the near side."** Specifying the grip reduces the model's freedom to invent an implausible one.

## Grips that work

| Grip | Good for | Failure risk |
|---|---|---|
| Fingers wrapped, thumb front | Jars, bottles, cans, cups | Low |
| Pinched between thumb and forefinger | Small items, jewellery | Medium |
| Flat palm, object resting | Small flat items | Low |
| Fingers cradling from below | Bowls, delicate items | Medium |
| Holding by a handle | Mugs, tools, bags | Low |
| Two hands presenting | Larger items | **High** |
| Fingers splayed | Nothing. Avoid | **Very high** |

**Fingers wrapped is the safest grip** and it works for most product shapes. Pinching is riskier because the fingertips are fully visible and they're where errors concentrate.

## Check every hand image

Non-negotiable, and quick once you know the list.

```
[ ] Count the fingers. Four plus a thumb
[ ] Thumb on the correct side for that hand
[ ] Joints bend in plausible directions
[ ] No finger merging into or passing through the product
[ ] Fingernails present and plausible, not smeared
[ ] Knuckle spacing even
[ ] Wrist connects plausibly to the forearm
[ ] Hand scale correct relative to the product
[ ] Skin texture natural, not waxy
```

**Count the fingers explicitly.** It sounds absurd and it's the check people skip because the image looks fine at a glance.

`analyze_image` (1 credit) asked "how many fingers are visible on the hand in this image?" is a genuinely useful automated check on a batch. See `product-image-qa-review`.

## Fixing rather than regenerating

A hand that's wrong in one place doesn't need a new generation.

```
edit_image(
  image=IMAGE,
  mask=HAND_REGION,
  prompt="A natural adult hand with four fingers and one thumb, "
         "fingers wrapped around the product, plausible joints, "
         "even knuckle spacing, natural skin texture. Nothing else "
         "changed."
)
```

Inpainting the hand preserves the composition and the product, which is what you liked. Regenerating gambles both. See `inpainting-product-fixes`.

## Diversity and representation

If hands appear across a catalogue, they should reflect the customer base rather than defaulting to a single skin tone, age or gender presentation.

- **Vary skin tone, age and hand type** deliberately across a set
- **State it in the prompt.** Unspecified, the model produces a narrow default
- **Keep the product, light and framing identical** so the set stays consistent
- **Don't tokenise.** Vary genuinely across the catalogue rather than adding one image

```
seed=8812  "...held in one adult hand, deeper skin tone..."
seed=8812  "...held in one adult hand, mid skin tone..."
seed=8812  "...held in one older adult hand, visible age..."
```

Same seed, same everything else. One variable.

## Nails, jewellery and skin

Details that undermine an otherwise good shot:

- **Manicured nails** date an image and narrow who it speaks to. Natural, short nails are safer
- **Rings and watches** compete with the product and can imply an unrelated brand. Prompt them out: "no jewellery on the hand"
- **Heavy retouching** produces plastic skin. Some texture and visible pores read as real
- **Nail polish colour** should be neutral or absent unless it's deliberate

## Don't

- **Don't generate splayed fingers.**
- **Don't generate two hands** unless you have to.
- **Don't omit "hand at natural adult scale."** It misinforms about size.
- **Don't skip the finger count.**
- **Don't regenerate** when inpainting the hand will do.
- **Don't let one skin tone default** across a whole catalogue.
- **Don't leave rings or a watch** in frame competing with the product.
- **Don't generate the hand shot** for a critical listing image. Photograph it.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-scale-reference`, `inpainting-product-fixes`, `lifestyle-product-photography`, `model-wearing-product`, `product-image-qa-review`, `background-replacement-scenes`, `ugc-selfie-style-photos`
