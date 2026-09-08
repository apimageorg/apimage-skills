---
name: jewelry-reflective-products
description: Handle the hardest product category — jewellery, watches, glass, chrome and anything transparent or mirrored — where cutouts halo and reflections come back wrong. Use whenever the user is photographing jewellery, watches, glassware, chrome, mirrors, or any transparent or highly reflective product.
---

# Reflective and Transparent Products

This is the category where every default workflow fails. Cutouts halo, backgrounds show through, reflections come back showing the old scene, and relighting produces highlights that don't correspond to any light source.

The reason is structural: for a transparent or mirrored product, **the background is part of the product's appearance.** There's no clean boundary between subject and scene, so tools that assume one produce artefacts.

## Why the standard workflow breaks

| Tool | Normal product | Reflective / transparent |
|---|---|---|
| `remove_background` | Clean cutout | Halos, lost transparency, edge fringing |
| `replace_background` | Relit plausibly | Reflections show the **old** scene |
| `edit_image` relight | Predictable | Highlights appear with no source |
| Reference-based generation | Accurate | Refraction and caustics invented |
| Upscaling | Adds texture | Adds fake sparkle |

**Reflections showing the old background is the tell.** A chrome watch case composited into a kitchen still reflecting the studio's white sweep reads as fake instantly.

## The workflow that actually works

Photograph, then adjust — rather than generate, then fix.

```
1. Real photograph on the surface you want          the source of truth
2. edit_image relight / grade                        modest adjustment only
3. edit_image inpaint the edges and reflections      targeted repair
4. Composite in an editor where control is needed
```

**For this category, prefer editing a real photograph over generating a scene.** The refraction, the caustics, the specular highlights and the reflections are physically complex, and a photograph already has them correct.

Generation is for the *scene around* a photographed product, in modest amounts.

## If you must cut out

```
1. remove_background                    2 credits
2. Composite onto white AND black       → find the damage
3. Mask the edges and any lost transparency
4. edit_image inpaint per region        → repair
5. Re-check on both backgrounds
```

Expect the inpainting pass as a **normal step**, not an exception. For a glass bottle, budget two or three targeted repairs.

```
edit_image(
  image=CUTOUT,
  mask=EDGE_REGION,
  prompt="Clean glass edge with natural refraction, no halo, no "
         "fringe. Transparent where the glass is transparent, "
         "showing the background through it. Nothing else."
)
```

**"Transparent where the glass is transparent"** is worth stating. `remove_background` frequently makes transparent areas opaque, which turns a glass bottle into a frosted one.

See `background-removal-workflow` and `inpainting-product-fixes`.

## Prompting for reflective materials

When generating a scene around a photographed reflective product, name what should be reflected.

```
generate_image(
  model="flux-2-max",
  reference_images=[WATCH_REFS],
  prompt="The same watch on a dark slate surface. Soft large "
         "diffused light source above and to the left, creating one "
         "long soft highlight along the polished case. Reflections "
         "on the case show the dark surroundings and the single "
         "light source, nothing else. Deep shadow on the right of "
         "the case. Product unchanged — same dial, hands, markings.",
  aspect_ratio="1:1",
  seed=8812
)
```

The clauses that matter:

- **"One long soft highlight"** — specify the highlight count and shape. Unspecified, you get scattered random speculars that correspond to nothing
- **"Reflections show the dark surroundings and the single light source, nothing else"** — this is what stops the old scene appearing
- **"Soft large diffused light source"** — hard small lights produce blown pinpoint highlights on polished metal
- **`flux-2-max`** — this is the category where the highest-fidelity model earns its cost. See `image-model-selection`

## Lighting principles for the category

Real product photographers solve reflection with light shaping, and the same vocabulary works in prompts.

| Material | Light |
|---|---|
| Polished metal, chrome | Large soft source, one clear highlight, dark surround |
| Glass, transparent | Backlit or edge-lit, dark surround, bright rim |
| Gemstones | Directional, to create internal sparkle — but don't add sparkle that isn't there |
| Watch dials | Soft frontal to avoid glare on the crystal |
| Mirrors | Angled so they reflect something intentional |

**A dark surround is the key insight for chrome.** Polished metal reflects its environment, so a bright white studio produces a product that looks like a white blob. A dark surround with one controlled highlight is what makes the form read.

## Accuracy risks specific to this category

**Added sparkle.** Models add specular glints and gemstone fire that aren't in the real product. For jewellery that's a material misrepresentation — the customer expects the stone to catch light like that.

**Invented facets and settings.** Generated gemstone facets, prong settings and engraving are fabrications. Photograph these.

**Metal colour shift.** Rose gold, yellow gold and brass shift easily under a grade. Verify against the physical piece.

**Carat, size and stone count.** Never let a generation change these. They're specification, not styling.

**Hallmarks and stamps.** Regulatory markings must be photographed, never generated.

For jewellery specifically, the safest position is: **photograph the piece; generate only the background and the surface it sits on.** See `product-image-qa-review`.

## Scale is unusually important here

Jewellery photographed alone reads as any size. A ring shot in isolation could be a signet or a delicate band.

- Include a hand, a wrist or a familiar object in at least one gallery image
- State dimensions in the copy
- Use a consistent scale convention across the catalogue

See `product-scale-reference` and `product-in-hand-shots`.

## The QA pass

```
[ ] Reflections show the current scene, not the old one
[ ] Highlight count and position correspond to the stated light
[ ] Transparent areas are actually transparent
[ ] No halo — checked on white AND black
[ ] Metal colour matches the physical piece
[ ] No added sparkle or fire
[ ] Facets, settings and engraving are photographed, not generated
[ ] Hallmarks legible and correct
[ ] Stone count and size unchanged
```

## Don't

- **Don't generate gemstone facets, settings or engraving.** Photograph them.
- **Don't add sparkle.** It's a material misrepresentation.
- **Don't light chrome with a bright white surround.** Dark surround, one highlight.
- **Don't accept scattered random speculars.** Specify the highlight.
- **Don't let `remove_background` make glass opaque.**
- **Don't skip the inpainting pass.** It's normal here.
- **Don't generate hallmarks or stamps.**
- **Don't photograph jewellery without a scale cue** somewhere in the gallery.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-removal-workflow`, `inpainting-product-fixes`, `product-relighting`, `product-detail-macro`, `product-scale-reference`, `product-image-qa-review`
