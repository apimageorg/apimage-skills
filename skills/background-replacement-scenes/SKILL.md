---
name: background-replacement-scenes
description: Swap a product's background with replace_background, which relights the subject to match the new scene. Use whenever the user wants a product in a different setting, a new scene behind a product, seasonal or lifestyle variants, or a product moved from studio to context.
---

# Background Replacement

`replace_background` swaps the scene **and relights the subject to match it**, for 3 credits. That relighting is the whole point — it's what separates a plausible composite from a cutout pasted on a photo.

It's the fastest way to turn one product shot into ten contexts.

## The call

```
replace_background(
  image="<product photo or cutout>",
  prompt="Pale oak kitchen counter, soft morning light from a window "
         "on the left, blurred kitchen behind, warm neutral grade, "
         "shallow depth of field"
)
→ product on the new scene, relit
```

3 credits. Describe the **scene and the light**, not the product — the product comes from the input image.

## Why the relighting matters

A naive composite fails in a specific, recognisable way: the product's lighting doesn't match the scene's. Sun coming from the right, product lit from the left, no contact shadow, hard edges. Everyone reads it as fake without being able to say why.

`replace_background` handles the light direction, the colour temperature and the contact shadow together. That's the difference between an asset you can put on a listing and one that looks like a mistake.

| | Manual composite | `replace_background` |
|---|---|---|
| Light direction | Mismatched unless you fix it | Matched |
| Colour temperature | Mismatched | Matched |
| Contact shadow | You add one, approximately | Generated plausibly |
| Edge integration | Hard, obvious | Softer |
| Cost | Editor time | 3 credits |

## Describe the light, not just the place

The most common weak prompt names a location and stops. Light is what makes a scene read.

**Weak:**
```
kitchen counter background
```

**Strong:**
```
Pale oak kitchen counter, soft directional morning light from a
window on the left, gentle falloff to the right, blurred kitchen
interior behind at f/2.8, warm neutral grade, subtle contact shadow
under the product
```

The elements worth stating every time:

| Element | Example |
|---|---|
| Surface | pale oak, white marble, brushed concrete, linen |
| Light direction | from the left, from behind, overhead, wraparound |
| Light quality | soft diffused, hard directional, overcast, golden hour |
| Colour temperature | warm, neutral, cool |
| Background depth | blurred at f/2.8, sharp, distant |
| Grade | warm neutral, muted, high contrast |
| Shadow | subtle contact shadow, long soft shadow, none |

## Cut out first, where it helps

`replace_background` works on a normal photo, and it works *better* on a clean cutout — the subject boundary is unambiguous.

```
1. remove_background      2 credits  → clean cutout
2. replace_background     3 credits  → the scene, relit
```

5 credits total, and materially better separation on anything with a busy original background. Skip the cutout when the original is already on plain white. See `background-removal-workflow`.

## Scene libraries

The efficient pattern: define a set of scenes once, apply them to every product.

```python
SCENES = {
    "kitchen":  ("Pale oak counter, soft morning light from the left, "
                 "blurred kitchen behind, warm neutral grade."),
    "bathroom": ("White marble surface, bright even daylight, "
                 "blurred tiled wall behind, cool neutral grade."),
    "desk":     ("Matte dark wood desk, soft overhead light, "
                 "blurred office behind, neutral grade."),
    "outdoor":  ("Weathered timber table, dappled afternoon sun, "
                 "blurred foliage behind, warm grade."),
    "studio":   ("Seamless mid-grey backdrop, soft wraparound light, "
                 "subtle contact shadow, neutral grade."),
}

for name, scene in SCENES.items():
    replace_background(image=cutout, prompt=scene)
```

Then save the ones that work as background brand assets:

```
create_brand_asset(type="background", ...)     # free
```

Now a new product gets the whole scene set for 3 credits each, in the same visual language as everything else in the catalogue. That's what makes a catalogue look art-directed. See `product-photo-consistency`.

## Where it struggles

- **Transparent products.** The background shows *through* the product, and a replaced background behind glass rarely refracts correctly
- **Highly reflective products.** Chrome and mirrors should reflect the new scene, and usually reflect the old one or nothing
- **Products with complex edges** — mesh, fine hair, lace
- **Products that were originally lit very hard** in a direction that fights the new scene
- **Scenes requiring the product to be occluded** by something in front of it

For transparent and reflective products, inspect and repair with `edit_image` inpainting. See `inpainting-product-fixes` and `jewelry-reflective-products`.

## Check the physics

The review pass, and it's quick once you know what to look for.

```
[ ] Light direction on the product matches the scene's light source
[ ] Colour temperature consistent — no warm product on a cool set
[ ] Contact shadow present, and falling the right way
[ ] Shadow softness matches the light quality (hard light = hard shadow)
[ ] Product scale plausible against the surface and background
[ ] Reflections on the product show the new scene, not the old
[ ] No halo or fringe at the product edge
[ ] Product colour still accurate to the real product
```

**Shadow direction is the giveaway.** If the light comes from the left, the shadow falls right. A shadow falling toward the light source reads as wrong immediately even to people who don't know why.

**Colour accuracy is the commercial risk.** A warm scene grade shifts your product's colour, and a customer receiving something that doesn't match the listing is a return. Keep a neutral render for the listing itself and use graded scenes for lifestyle. See `product-image-qa-review`.

## Cost

| Operation | Credits |
|---|---|
| `remove_background` | 2 |
| `replace_background` | 3 |
| Both | 5 |
| Prompting a scene via `generate_image` instead | 1-9, and less reliable |

**Use the dedicated tool rather than prompting a scene change** through `generate_image`. It's cheaper and the relighting is better.

Rate limit on background tools is **30 requests/minute**. See `product-photo-batch-pipeline`.

## Don't

- **Don't just name a location.** Describe the light.
- **Don't describe the product.** It comes from the input.
- **Don't skip the cutout** on a busy original.
- **Don't prompt a scene swap** through `generate_image` when this tool exists.
- **Don't accept a shadow falling the wrong way.**
- **Don't use a warm-graded scene** for the listing image. Neutral for listings.
- **Don't expect transparent or chrome products** to work first time.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-removal-workflow`, `lifestyle-product-photography`, `product-relighting`, `seasonal-product-styling`, `inpainting-product-fixes`, `product-photo-consistency`, `ugc-lifestyle-photos`
