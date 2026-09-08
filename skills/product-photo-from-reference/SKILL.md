---
name: product-photo-from-reference
description: Generate new product photography from a real product photo using reference_images, so the product stays accurate instead of being reinvented. Use whenever the user has a product photo and wants more shots, new angles, new scenes, or variations, or asks how to get consistent product imagery.
---

# Product Photos from a Reference

The core APImage image workflow: one real product photo in, many accurate variations out. It replaces a photoshoot for scenes, angles and settings — but only if the product stays the product.

Generating a product from a text description produces something that *resembles* yours. For commerce that's not a stylistic difference, it's advertising a thing you don't sell.

## Always start from the real photo

```
generate_image(
  model="flux-2-pro",
  reference_images=["<real product photo>"],
  prompt="The same bottle, on a pale oak table, morning window light "
         "from the left, shallow depth of field, warm neutral grade. "
         "Product unchanged — same label, same shape, same colour.",
  aspect_ratio="1:1",
  seed=8812
)
```

`reference_images` accepts **up to 4 for images**. Use all of them for anything where accuracy matters.

Three clauses doing work:

**"The same bottle"** rather than describing the bottle. The reference supplies the product; re-describing it competes with the reference and causes drift.

**"Product unchanged — same label, same shape, same colour."** It won't fully prevent drift but it measurably reduces it.

**Everything else** is the scene — surface, light, grade, depth. That's what you're actually generating.

## The reference photo decides the ceiling

A mediocre reference produces mediocre variations and no prompt fixes it.

**What makes a good reference:**
- Sharp, evenly lit, product filling most of the frame
- Label and logo fully legible
- Neutral or plain background, so the product is separable
- No heavy shadow obscuring form
- True colour — not a warm-graded lifestyle shot
- No motion blur, no compression artefacts

**Multiple angles beat one.** A front-on reference gives the model one view; front, three-quarter, side and back give it a product it can hold onto when the scene changes the angle.

If the reference is cluttered, `remove_background` (2 credits) first to isolate the product cleanly, then use the cutout as the reference. See `background-removal-workflow`.

## Save it once, use it forever

The step that turns this from a one-off into a pipeline.

```
create_brand_asset(type="product", ...)      # free
list_brand_assets(type="product")            # free
get_brand_asset(type="product", id="...")    # free
```

Every brand asset operation is free. Save the approved reference set the moment you approve it, and every future shot — image or video — starts from the same product.

That's what makes forty product images look like one catalogue instead of forty separate attempts. See `product-photo-consistency`.

## What generation is good at, and not

| Reliable | Risky | Don't |
|---|---|---|
| New backgrounds and surfaces | Rotating to an unseen face | Changing product design |
| New lighting and grade | Complex reflections | Adding features it doesn't have |
| Adding context objects | Legible small text on-pack | Altering the label copy |
| Depth of field changes | Transparent or liquid contents | Recolouring to a variant you don't sell |
| Scene and setting | Fine texture on fabric | Changing proportions |
| Hands holding it | Metallic and mirror finishes | Fixing a genuinely bad reference |

**Anything the reference doesn't show will be invented.** If your references are front-on only and the prompt asks for a rear three-quarter, the model makes up the back — and it will be wrong.

Either supply references covering the angles, or keep the generated angles within what the references support. See `camera-angle-variation`.

## Iterate cheaply, then lock

Image generation is 1-9 credits. This is the cheap half of the whole platform and where iteration belongs.

```
1. 3-4 unseeded generations   → find a composition and look you like
2. Note that seed              → lock it
3. Change ONE thing per gen    → surface, light, angle, grade
4. Log what worked
```

Locking the seed makes each change attributable. Without it every generation is a new roll and you can't tell whether the prompt edit helped. See `seed-locked-iteration`.

**Fixing the product in the image is far cheaper than fixing it in video.** If a downstream video clip has the wrong product, come back here — image iterations cost a fraction of video ones. See `video-credit-cost-management`.

## Colour accuracy

The failure mode that causes returns and complaints, and it's easy to miss.

Generated scenes apply a grade, and a warm grade shifts your product's colour. A navy shirt rendered in golden-hour light reads as charcoal, and the customer receives something that doesn't match the listing.

- **Check colour against the real product**, not against the reference photo (which has its own grade)
- **State the grade neutrally** for listing images: "neutral white balance, accurate colour"
- **Keep lifestyle grading for lifestyle shots** and use neutral renders for the listing itself
- **Never recolour a product** into a variant you don't sell

See `white-background-ecommerce` and `product-image-qa-review`.

## Cost

| Step | Cost |
|---|---|
| `enhance_prompt` | free |
| `remove_background` on the reference | 2 credits |
| `generate_image` | 1-9 credits each |
| `create_brand_asset` | free |
| Polling and history | free |
| `analyze_image` for QA | 1 credit |

A full set of scene variations from one reference costs a handful of credits. That's the argument for doing this rather than reshooting.

## Don't

- **Don't generate a product from a text description.** Use the real photo.
- **Don't describe the product** in a prompt that has references. Describe the scene.
- **Don't ask for angles the references don't cover.**
- **Don't use a cluttered reference.** Cut it out first.
- **Don't skip `create_brand_asset`.** It's free and it's what makes the set consistent.
- **Don't judge colour against the reference.** Judge against the real product.
- **Don't recolour into a variant you don't sell.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-consistency`, `background-removal-workflow`, `background-replacement-scenes`, `camera-angle-variation`, `image-model-selection`, `product-image-qa-review`, `ugc-selfie-style-photos`
