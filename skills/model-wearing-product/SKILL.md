---
name: model-wearing-product
description: Generate on-model product imagery with a consistent synthetic model, and handle the likeness, fit and body-representation constraints. Use whenever the user wants a model wearing or using a product, on-model apparel shots, or a recurring person across a catalogue.
---

# On-Model Product Imagery

On-model shots communicate fit, scale, drape and desire in a way a flat product shot can't. They're also where the most constraints stack up: likeness rights, fit accuracy, body representation rules and the model consistency problem.

## Build a model, don't license one

Same argument as `ai-avatar-presenter`, and it applies more sharply here because apparel imagery gets scrutinised.

| | Real model | Stock image | Generated model |
|---|---|---|---|
| Likeness permission | Contract and release | Licence often **excludes** synthetic use | Yours |
| Consistency across catalogue | Rebooking | One image only | Unlimited |
| Cost per new product | Day rate | New licence | Credits |
| Appears on competitors' sites | No | **Likely** | No |

**Stock portraits are frequently a licence breach as an AI reference.** Most stock licences either prohibit use as input to AI systems or prohibit depicting the model as endorsing a product. Both apply to on-model product imagery.

Generate a model, save them, reuse them.

## Build the reference set

```
1. generate_image  → iterate the model at 1-9 credits until right
2. Generate 3-4 more views at the same seed, varying only the angle
3. create_brand_asset(type="character", ...)   → free
4. Every product shot references the set
```

```
generate_image(
  model="flux-2-pro",
  prompt="Full-length portrait of an adult woman, mid-thirties, "
         "medium build, shoulder-length dark hair, neutral "
         "expression, standing straight, arms relaxed. Plain "
         "mid-grey studio background, soft even lighting. Wearing "
         "plain fitted neutral base layers. Sharp focus.",
  aspect_ratio="3:4",
  seed=8812
)
```

**Plain neutral base layers** in the reference matters — it gives the model a clean body form to dress rather than an existing garment competing with the one you're adding.

See `character-consistency-video` for the asset mechanics; they're identical.

## Fit accuracy is the claim

This is the on-model-specific accuracy line, and it's the one that drives returns.

**Not acceptable:**
- Generating a fit the garment doesn't have — a loose shirt rendered fitted
- Generating drape the fabric doesn't produce — stiff cotton rendered as flowing silk
- Showing a size that isn't the size worn
- Reshaping the model's body to flatter the garment
- Implying the garment fits all body types when the image shows one

**Acceptable:**
- The actual garment on a consistent model, with the size stated
- Normal styling — tucked, sleeves rolled — if it's how it's shown
- Scene and background generation around an accurate garment

**Practical position: photograph the garment on a real model for the fit-critical image, then use `replace_background` for scene variants.** 3 credits per setting with the fit and drape intact. That's the workflow that's both compliant and cheap.

Generate on-model imagery for scene variety, not for fit representation. See `apparel-product-photography`.

## Body representation rules

An area with real regulation in several markets.

- **Body-altering retouching in advertising requires disclosure** in a growing number of jurisdictions. Generating a body is arguably a stronger version of the same thing
- **Don't reshape the model** between shots to suit each garment
- **Don't generate a body type that misrepresents** how the garment fits
- **Show a range of body types** across a catalogue if you claim a range of sizes — and generate them honestly rather than scaling one model
- **Health and diet categories** have additional restrictions on body imagery

Check the rules in the target market. This is changing quickly and the direction of travel is toward more disclosure.

## Keeping the model consistent

```
generate_image(
  model="flux-2-pro",
  reference_images=[MODEL_REF_1, MODEL_REF_2, MODEL_REF_3, GARMENT_REF],
  prompt="The same woman wearing the same shirt, standing straight, "
         "three-quarter view, arms relaxed. Plain mid-grey studio "
         "background, soft even lighting from the upper left. "
         "Garment unchanged — same colour, fit and detail.",
  aspect_ratio="3:4",
  seed=8812
)
```

`reference_images` accepts **up to 4 for images** — so three model views plus the garment, or two of each. Prioritise by which needs more fidelity.

**Never describe the model's appearance in the prompt.** The references handle identity. Describing it again competes with them and produces drift. Describe pose, framing and light only.

## Poses that work

| Pose | Shows | Risk |
|---|---|---|
| Standing straight, arms relaxed | Fit, length, drape | Low |
| Three-quarter turn | Silhouette, side profile | Low |
| Walking, mid-stride | Movement, drape | Medium |
| Seated | Fit while sitting — genuinely useful | Medium |
| Arms raised | Sleeve length, riding up | High |
| Hands in pockets | Casual, natural | Medium — hands |
| Detail crop, no face | Fabric, hardware, fit at a point | **Low, and underused** |

**Detail crops with no face are the safest and most under-used on-model shot.** A cropped torso showing the fit through the waist, or a sleeve at the cuff, has no face and no hands to get wrong, and it answers a real question.

## The QA pass

```
[ ] Same model as the rest of the catalogue
[ ] Hands: correct digits, plausible joints
[ ] Face: no distortion, no uncanny asymmetry
[ ] Garment fit matches the real garment
[ ] Garment colour verified against the physical item
[ ] Pattern and print photographed, not generated
[ ] Body proportions consistent with previous shots
[ ] No implied endorsement by an apparently real person
[ ] Size worn stated in the copy
```

**Pattern and print must be photographed.** Generated patterns reinvent the motifs, and on a worn garment that's immediately visible. See `apparel-product-photography`.

## Disclosure

- **Label AI-generated imagery** where the platform or market requires it
- **Don't present a generated model as a real customer** giving a testimonial. That's a false endorsement
- **Don't imply the model owns or endorses** the product beyond wearing it
- **Some marketplaces prohibit synthetic imagery for main listing images** — check before publishing. See `marketplace-image-compliance`

## Don't

- **Don't use a stock portrait** as a model reference without checking the licence.
- **Don't use a real person's likeness** without a release covering synthetic use.
- **Don't generate a fit the garment doesn't have.**
- **Don't reshape the model's body.**
- **Don't describe the model's appearance** in a prompt with references.
- **Don't generate printed patterns.**
- **Don't present a generated model as a real customer.**
- **Don't rely on generated on-model shots** for fit representation. Photograph those.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`apparel-product-photography`, `ai-avatar-presenter`, `character-consistency-video`, `product-in-hand-shots`, `product-image-qa-review`, `marketplace-image-compliance`
