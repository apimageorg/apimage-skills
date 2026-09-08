---
name: product-detail-macro
description: Generate extreme close-up detail shots showing material, texture, stitching and hardware — the quality signals customers look for. Use whenever the user needs macro shots, texture close-ups, material detail, hardware or construction detail, or wants to show product quality.
---

# Detail and Macro Shots

Detail shots answer the question a product photo can't: what is this actually made of? For anything sold on quality — apparel, leather, furniture, tools, jewellery — the macro shot is where the purchase decision often gets made.

They're also cheap, quick, and among the most reliable generated images, because there's no composition, no perspective and no scale to get wrong.

## What a detail shot is for

| Signal | Shot |
|---|---|
| Material quality | Extreme close-up on the surface |
| Construction quality | Stitching, seams, joins |
| Hardware quality | Zips, buckles, clasps, hinges |
| Finish | Edge treatment, polish, coating |
| Texture | Weave, grain, nap, knurl |
| Print quality | Label print, embossing, engraving |
| Wear resistance | Reinforcement points |

**Stitching is the highest-signal detail for anything sewn.** Stitch density, evenness and thread quality are what a customer reads as "well made", and it's the shot most catalogues omit.

## The call

```
generate_image(
  model="flux-2-max",
  reference_images=[MATERIAL_REFS],
  prompt="Extreme close-up on the leather surface and stitched edge, "
         "filling the frame. Visible irregular natural grain, "
         "individual stitches evenly spaced with visible thread "
         "twist. Raking side light from the left revealing surface "
         "texture, shallow depth of field falling off toward the "
         "back. Material unchanged.",
  aspect_ratio="1:1",
  seed=8812
)
```

Three things:

**`flux-2-max`.** This is the category where the highest-fidelity model earns its cost — the whole image is fine detail, so a lower-fidelity model produces a soft or waxy surface, which defeats the shot. See `image-model-selection`.

**Raking side light.** Texture is revealed by light at a shallow angle across the surface. Frontal light flattens it. "Raking side light" is the single most important phrase for a macro shot.

**Named texture specifics.** "Irregular natural grain", "individual stitches", "visible thread twist". Generic "high quality leather texture" produces a uniform pattern that reads as fake.

## Texture vocabulary that works

| Material | Prompt language |
|---|---|
| Leather | "irregular natural grain, visible pores, slight sheen variation" |
| Denim | "visible twill weave running diagonally, defined wash variation" |
| Knit | "consistent rib direction, individual loops visible" |
| Wood | "open grain running vertically, visible medullary rays" |
| Brushed metal | "fine parallel brush marks, directional sheen" |
| Ceramic | "subtle glaze pooling, slight surface irregularity" |
| Stone | "irregular veining, matte with slight variation" |
| Paper | "visible fibre texture, deckled edge" |

**Direction matters.** Grain, weave and brush marks all run a direction, and "running vertically" or "diagonally" prevents the model producing a pattern that drifts across the frame.

## The accuracy risk

Detail shots are where fabrication is most tempting and most consequential, because a macro shot is read as evidence of quality.

**Never generate:**
- **Stitch density or quality** that the product doesn't have
- **A material texture** the product isn't made of
- **Engraving, embossing or hallmarks** — these are text and specification
- **Reinforcement or construction detail** that doesn't exist
- **A finish quality** better than the real item

**Photograph the detail; generate the light.** A close-up photograph of the real stitching, relit with `edit_image` to reveal texture better, is honest. A generated stitch pattern is a quality claim about a product you didn't examine.

This is the strictest accuracy line in product photography, because a detail shot is specifically a claim about how well the thing is made. See `product-image-qa-review`.

## Depth of field

Macro shots have very shallow depth of field in reality, and reproducing that is what makes them read as macro rather than as a crop.

```
"shallow depth of field, sharp at the stitch line, falling off
 rapidly toward the back of the frame"
```

- **Name where the focus plane is.** "Sharp at the stitch line" rather than just "shallow depth of field"
- **Rapid falloff** is what says macro. Gentle falloff says normal lens
- **Don't have everything sharp.** A fully-sharp macro looks like an upscaled crop

## Lighting for texture

| Light | Reveals |
|---|---|
| **Raking side light** | Surface texture, grain, weave — the default |
| Backlight through the material | Translucency, weave openness |
| Soft frontal | Colour accuracy, minimal texture |
| Hard directional | Sheen, polish, specular character |
| Diffused overhead | Flat, minimal shadow — avoid for texture |

**Raking light for texture; soft frontal for colour.** Those are different shots and you often want both — one to show what it feels like, one to show what colour it is.

## Detail shots as a set

For a product where quality is the argument, four or five detail shots is a strong gallery.

```python
DETAILS = [
    "the stitched edge, individual stitches visible",
    "the leather surface grain, filling the frame",
    "the zip pull and its stitching",
    "the interior lining and its seam",
    "the embossed logo at a raking angle",
]
LIGHT = ("Raking side light from the left revealing surface texture, "
         "shallow depth of field with rapid falloff.")

for d in DETAILS:
    generate_image(model="flux-2-max", reference_images=REFS,
                   prompt=f"Extreme close-up on {d}. {LIGHT} "
                          f"Material unchanged.",
                   aspect_ratio="1:1", seed=8812)
```

Same light clause verbatim, same seed. See `product-photo-consistency`.

## Scale in a macro shot

A macro shot has no scale, and sometimes that matters — a customer can't tell whether they're looking at a 2mm or a 20mm stitch.

- **Include a scale cue** in at least one detail shot where it matters — a thread, a fingertip at the frame edge
- **State the dimension** in the caption or copy
- **Keep macro shots consistent in magnification** across a catalogue, so customers can compare

See `product-scale-reference`.

## Don't

- **Don't generate stitch quality, engraving or hallmarks.** Photograph them.
- **Don't use a lower-fidelity model.** The whole image is detail.
- **Don't use frontal light for texture.** Raking side light.
- **Don't write "high quality texture."** Name the specific surface character.
- **Don't omit the grain or weave direction.**
- **Don't have everything sharp.** Name the focus plane and the falloff.
- **Don't imply a construction quality** the product doesn't have.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`apparel-product-photography`, `jewelry-reflective-products`, `product-relighting`, `product-upscaling-4k`, `product-scale-reference`, `product-image-qa-review`, `ugc-asmr-sensory-format`
