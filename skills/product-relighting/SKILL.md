---
name: product-relighting
description: Change the lighting on an existing product photo with edit_image relighting and colour grading, without regenerating the shot. Use whenever the user wants to fix bad lighting, change light direction or mood, warm or cool an image, or match a photo to a set's look.
---

# Relighting Product Photos

`edit_image` supports relighting and colour grading, which means you can change the light on a shot you already like rather than rolling a new generation and losing the composition.

That distinction is the whole value: **relighting preserves what worked; regenerating gambles it.**

## `edit_image` vs regenerating

```
Regenerate:   new prompt, new roll — composition, angle and framing all change
edit_image:   same image, the lighting changed
```

If the composition is right and only the light is wrong, `edit_image` is the correct tool. Regenerating with "…but warmer" gives you a different shot that also happens to be warmer.

Same logic as upscaling, angle adjustment and inpainting — the edit tools exist so you don't have to re-roll a good frame. See `image-model-selection`.

## What relighting fixes

| Problem | Relight to |
|---|---|
| Flat, shadowless, lifeless | Soft directional light with visible falloff |
| Harsh on-camera flash | Diffused light, softer shadow |
| Mixed colour temperature | One consistent temperature |
| Wrong mood for the channel | Warm and soft, or cool and clinical |
| Doesn't match the rest of the set | The set's look, stated verbatim |
| Product form reads badly | Light angled to reveal shape |
| Dull, no dimension | Slight rim or edge light |

**Flat lighting is the most common fault in supplied product photography** and the easiest to fix. A product lit from directly front-on has no shadow, so it has no perceived form. Adding a direction gives it shape.

## Describe light like a photographer

The prompt quality is the output quality here, and vague direction produces vague results.

**Weak:**
```
better lighting, professional, nice
```

**Strong:**
```
Relight: soft directional light from the upper left at about 45
degrees, gentle falloff to the lower right, subtle fill from the
right to keep shadow detail, soft contact shadow beneath, neutral
white balance. Keep the product, composition and framing unchanged.
```

The elements worth naming:

| Element | Options |
|---|---|
| Direction | upper left, side, behind, overhead, front |
| Angle | 45 degrees, low, raking, top-down |
| Quality | soft diffused, hard directional, wraparound |
| Falloff | gentle, rapid, even |
| Fill | subtle fill opposite, none, strong |
| Temperature | warm, neutral, cool, and Kelvin if you like |
| Shadow | soft contact, long, hard-edged, none |
| Accents | rim light, edge light, specular highlight |

**Always include "keep the product, composition and framing unchanged."** Without it the edit drifts beyond the lighting.

## The looks worth having as presets

Most catalogues need three or four looks, reused. Define them once, verbatim.

```python
LOOKS = {
    "clean_studio": ("Soft wraparound studio light, minimal shadow, "
                     "neutral white balance, even exposure, no grade."),
    "warm_natural": ("Soft directional daylight from the upper left, "
                     "warm neutral grade, gentle falloff, soft "
                     "contact shadow."),
    "moody_premium": ("Single hard light from the side, deep shadow "
                      "on the opposite face, cool grade, dark "
                      "background falloff."),
    "bright_airy":  ("Bright even daylight, high key, minimal shadow, "
                     "slightly cool white balance, airy."),
}
```

Then save the ones you settle on:

```
create_brand_asset(type="preset", ...)     # free
```

**Preset brand assets are the underused asset type.** A saved look applied across a catalogue is what makes forty images read as one brand rather than forty attempts. See `product-photo-consistency`.

## Matching a photo into an existing set

The most common real task: one supplied photo that doesn't match the other fifty.

```
1. Pick a reference image from the set that works
2. analyze_image on it (1 credit) → get the lighting described
3. Use that description verbatim as the relight prompt
4. Compare side by side, adjust once
```

`analyze_image` describing your own good image is a genuinely useful trick — it produces a lighting description in the right vocabulary, which you then apply to the odd one out.

## Where relighting can't save it

Be realistic. Relighting redistributes what's in the image; it doesn't add information.

| Fixable | Not fixable |
|---|---|
| Flat lighting | Blown-out highlights with no detail |
| Wrong temperature | Crushed blacks with no detail |
| Wrong direction | Motion blur |
| Slightly under or over exposed | Out-of-focus product |
| Dull grade | Wrong product angle → use `camera-angle-variation` |
| Missing contact shadow | Low resolution → use `product-upscaling-4k` |

**Blown highlights and crushed blacks are gone.** The data isn't there. Relighting a blown white label produces a differently-lit blown white label.

For those, either reshoot the reference or generate a new shot from a better reference. See `product-photo-from-reference`.

## Colour accuracy warning

Relighting changes colour, and for commerce that's a risk.

- **Keep a neutral version for the listing image.** Graded versions are for lifestyle
- **Check the product colour against the real product** after relighting, not against the source photo
- **A warm grade makes navy read as charcoal**, and the customer returns it
- **Never relight into a colour you don't sell**

See `white-background-ecommerce` and `product-image-qa-review`.

## Cost

`edit_image` bills generation credits. Practical implications:

- It's comparable to generating, so it's not free — but it **preserves the composition**, which regenerating doesn't
- Iterate the light on a low resolution, then apply the final look at full resolution
- `enhance_prompt` is free — use it to fill out a thin lighting description
- `analyze_image` is 1 credit and worth it for extracting a look from a reference

## Don't

- **Don't regenerate** when only the light is wrong.
- **Don't write "better lighting."** Name direction, quality, falloff and temperature.
- **Don't omit "keep composition and framing unchanged."**
- **Don't try to recover blown highlights or crushed blacks.**
- **Don't paraphrase the look** between images in a set. Copy it verbatim.
- **Don't ship a graded render** as the main listing image.
- **Don't judge colour against the source photo.** Judge against the product.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-replacement-scenes`, `product-photo-consistency`, `camera-angle-variation`, `product-upscaling-4k`, `inpainting-product-fixes`, `image-model-selection`, `ugc-asmr-sensory-format`
