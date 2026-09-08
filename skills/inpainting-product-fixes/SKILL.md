---
name: inpainting-product-fixes
description: Repair specific regions of a product image with edit_image inpainting and erasing via masked regions, instead of regenerating the whole shot. Use whenever the user wants to remove an object, fix a blemish, repair a bad edge, erase a reflection, or change one part of an image.
---

# Inpainting and Erasing

`edit_image` supports **inpainting and erasing via masked regions**. That means you can fix one part of an image and leave the rest untouched — which is almost always what you actually want.

The alternative is regenerating, which fixes the problem and changes everything else you liked.

## When to inpaint rather than regenerate

| Problem | Inpaint |
|---|---|
| A stray object in the frame | Erase it |
| A halo left by background removal | Repair the edge |
| Dust, scratch, fingerprint on the product | Remove it |
| An unwanted reflection | Erase or replace |
| A cable, tag or sticker that shouldn't be there | Erase |
| A blemish on a model's skin | Retouch |
| A distorted small area from a generation | Repair locally |
| A logo you don't have rights to in the background | Erase |

**Anything localised.** If the fault occupies less than roughly a quarter of the frame, inpainting is the cheaper and safer route.

Regenerate instead when the composition, angle or lighting is wrong — those aren't local problems. See `product-relighting` and `camera-angle-variation`.

## The call

```
edit_image(
  image="<the image>",
  mask="<mask marking the region to change>",
  prompt="Clean pale oak surface continuing the existing wood grain, "
         "matching the surrounding light and shadow. Nothing else."
)
```

Two things decide the result: the mask, and how specific the prompt is about what should be *there instead*.

## Mask discipline

The mask matters more than the prompt.

**Mask slightly beyond the fault.** A mask tight to the object's edge leaves the object's shadow and its colour fringe behind. Give it a small margin so the fill has surrounding context to blend into.

**Don't over-mask.** Masking half the image to remove one small object invites the model to reinvent things you wanted kept.

**Soft edges where the surroundings are soft**, hard edges where they're hard. A hard-edged mask across a blurred background produces a visible seam.

**Separate masks for separate faults.** Two objects on opposite sides of the frame are two edits, not one mask with two blobs — a single prompt can't describe two different fills well.

## Describe what should be there

The most common weak prompt says what to remove. Say what should replace it.

**Weak:**
```
remove the cable
```

**Strong:**
```
Clean pale oak surface continuing the existing wood grain in the same
direction, matching the surrounding light falloff and shadow density.
No objects.
```

The elements worth stating:

- **The surface or material** that should continue
- **The direction of any pattern** — grain, weave, tiling
- **The lighting** it should match
- **"No objects"** or "nothing else", to stop the model adding something

**Continuity of pattern is where inpainting most visibly fails.** Wood grain running the wrong way, tile lines not aligning, fabric weave at a different angle. Naming the direction fixes most of it.

## Repairing background-removal edges

The most frequent real use. `remove_background` leaves halos on transparent, reflective and fine-detailed products.

```
1. remove_background            2 credits
2. Composite onto white AND black → find the halo
3. Mask the affected edge
4. edit_image inpaint → "clean product edge, no fringe, no halo"
5. Re-check on both backgrounds
```

**The white-and-black test is what surfaces the problem.** A light halo is invisible against white and obvious against black. See `background-removal-workflow`.

For glass, chrome and mesh, expect to inpaint the edges as a normal step rather than an exception. See `jewelry-reflective-products`.

## What inpainting can't do

- **Recover blown highlights or crushed blacks.** The data isn't there — inpainting invents a plausible fill, which for a product surface is a fabrication
- **Fix an out-of-focus product.** Local sharpening isn't inpainting's job
- **Restore legible text.** A garbled label inpainted becomes differently garbled. Overlay real text, or photograph it
- **Change the product design.** That's misrepresentation, not retouching
- **Fix a large region convincingly.** Beyond about a quarter of the frame, regenerate

That third point matters: **never inpaint product text.** If a label is unreadable, the fix is a photograph or an overlay, not a generated approximation of your own copy.

## Where the retouching line sits

Inpainting is retouching, and retouching product imagery has limits — commercial and in some categories regulatory.

**Acceptable:**
- Removing dust, lint, fingerprints, sensor spots
- Removing a stray prop or cable
- Repairing a cutout edge or a generation artefact
- Removing a reflection of the studio
- Normal skin retouching on a model, within category norms

**Not acceptable:**
- Removing a genuine product defect that ships with the item
- Changing product proportions, colour or finish
- Removing required regulatory markings
- Altering a "before" image in a before-and-after
- Body reshaping on a model in markets where that requires disclosure

**The test:** does the edited image still accurately represent what the customer receives? Removing a fingerprint, yes. Removing the visible seam that's on every unit, no.

See `product-image-qa-review`.

## Verify locally and globally

```
[ ] Zoom to 200% on the edited region — seam visible?
[ ] Pattern continuity: grain, weave, tile alignment
[ ] Lighting and shadow match the surroundings
[ ] No new object introduced
[ ] Colour of the fill matches
[ ] Zoom out — does the whole image still read correctly?
[ ] Compare against the original side by side
```

**Check globally as well as locally.** A perfect local repair can subtly shift the whole frame's balance, and you only see it side by side with the original.

## Cost

`edit_image` bills generation credits. Practically:

- Cheaper than regenerating **and** it preserves the composition
- Iterate the mask at low resolution, apply at full resolution
- `analyze_image` (1 credit) on the result will sometimes flag an artefact
- `enhance_prompt` is free — useful for filling out a thin fill description

## Don't

- **Don't mask tight to the object.** Leave a margin.
- **Don't over-mask.** Big masks invite reinvention.
- **Don't say what to remove.** Say what should be there instead.
- **Don't ignore pattern direction.** It's the visible failure.
- **Don't inpaint product text.** Photograph or overlay it.
- **Don't inpaint over blown highlights.** The data is gone.
- **Don't remove a real defect** that ships with the product.
- **Don't check only locally.** Compare the whole frame against the original.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-removal-workflow`, `product-relighting`, `product-upscaling-4k`, `jewelry-reflective-products`, `product-image-qa-review`, `image-model-selection`
