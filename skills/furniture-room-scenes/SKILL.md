---
name: furniture-room-scenes
description: Place furniture and homeware in room scenes with correct scale, perspective and light. Use whenever the user is photographing furniture, homeware, rugs, lighting, appliances or anything that needs to be shown in a room.
---

# Furniture in Room Scenes

Furniture is bought on two questions: does it fit, and does it look right in a room like mine? Both are answered by imagery, and both are where generated room scenes go wrong.

The failures here are geometric rather than aesthetic — wrong scale, wrong perspective, impossible light — and they're the ones customers notice because they're mentally placing the item in their own space.

## Scale is the whole problem

A sofa photographed against a plain background could be a two-seater or a four-seater. In a room scene, the surrounding objects tell the customer the size — and if those objects are generated at the wrong scale, the image actively misinforms.

```
generate_image(
  model="flux-2-pro",
  reference_images=[SOFA_REFS],
  prompt="The same three-seater sofa in a room with a standard 2.4m "
         "ceiling. A normal-height doorway visible on the left for "
         "scale, a 40cm side table beside the sofa. Soft daylight "
         "from a window on the right. Wide-angle view from standing "
         "eye level, straight perspective, no distortion. Sofa "
         "unchanged — same proportions and upholstery.",
  aspect_ratio="4:3",
  seed=8812
)
```

The clauses doing the work:

**Named reference objects with dimensions.** A doorway, a plug socket, a standard side table. These are the objects a customer reads scale from, and stating their real size anchors the whole scene.

**Ceiling height.** "Standard 2.4m ceiling" prevents the model generating a cathedral space that makes your sofa look small.

**Camera height and perspective.** "From standing eye level, straight perspective" — unstated, the model picks, and it often picks a low dramatic angle that exaggerates size.

See `product-scale-reference`.

## Perspective errors to watch for

Room scenes have converging lines, and generated ones frequently get them wrong.

| Error | How it looks |
|---|---|
| Non-converging verticals | Walls lean, room feels unstable |
| Inconsistent vanishing point | Floor and ceiling lines disagree |
| Wrong horizon height | Furniture appears to float or sink |
| Barrel distortion | Straight edges bow |
| Furniture perspective ≠ room perspective | The item looks pasted in |

**The furniture-versus-room mismatch is the common one.** The sofa is rendered at one perspective and the room at another, so it reads as a composite even though it was generated as one image.

Prompt for it explicitly: *"furniture perspective matches the room's perspective, consistent vanishing point, verticals parallel."*

## Light has to be physically possible

- **Name the window position and time of day.** "Soft daylight from a large window on the right, mid-morning"
- **Shadows fall away from the light.** A shadow pointing at the window reads as wrong immediately
- **Shadow length matches the light height.** Low sun, long shadows
- **Interior fixtures should be lit if they're on** — a lamp in frame that's clearly on but casting no light is a tell
- **One dominant source.** Multiple competing sources produce contradictory shadows

For furniture, contact shadows matter more than usual: a sofa with no shadow where it meets the floor looks like it's hovering, and that single detail undoes an otherwise good render.

## Upholstery and materials

Same problem as apparel fabric, same answer.

| Material | Issue |
|---|---|
| Woven upholstery | Weave direction drifts |
| Velvet | Nap and sheen become plastic |
| Leather | Grain becomes uniform |
| Wood grain | Pattern reinvented, direction inconsistent |
| Patterned fabric | **Motifs reinvented** |
| Rattan and cane | Weave becomes mush |
| Marble and stone | Veining reinvented |

**Never generate a patterned upholstery or a specific wood grain.** Both are reinvented, and for a customer choosing between oak and walnut that's a material misrepresentation.

Photograph the piece; generate the room around it. See `product-photo-from-reference`.

## The efficient workflow

```
1. Photograph the piece, plain background, straight on and three-quarter
2. remove_background            2 credits  → clean cutout
3. create_brand_asset                      → free, reusable
4. replace_background           3 credits  → each room scene, relit
5. Verify scale, perspective and shadow per scene
```

`replace_background` relights the subject to the new scene, which handles the light-matching problem that manual compositing fails at. See `background-replacement-scenes`.

Build a room-scene library once and apply it to every piece:

```python
ROOMS = {
    "scandi_living": ("Pale oak floor, white walls, 2.4m ceiling, soft "
        "daylight from a large window on the right, minimal styling, "
        "a 40cm side table for scale."),
    "warm_traditional": ("Dark timber floor, warm painted walls, "
        "afternoon light from the left, a rug, a standard doorway "
        "visible for scale."),
    "modern_neutral": ("Polished concrete floor, grey walls, even "
        "diffused daylight, sparse styling, a normal plug socket "
        "visible at skirting height."),
}
create_brand_asset(type="background", ...)     # save the keepers
```

A plug socket at skirting height is an unglamorous but genuinely effective scale cue — everyone knows how big one is.

## Rugs, lighting and appliances

- **Rugs** need an overhead or high-angle shot for pattern, plus a room shot for scale. Pattern must be photographed
- **Lighting products** need to be shown lit, and the light they cast must be plausible for the fitting
- **Appliances** carry regulatory labels and energy ratings — photograph those faces. See `marketplace-image-compliance`
- **Flat-pack** benefits from an assembled room shot plus a packed-dimensions graphic

## The QA pass

```
[ ] Scale plausible against named reference objects
[ ] Ceiling height looks standard, not cathedral
[ ] Verticals parallel, one consistent vanishing point
[ ] Furniture perspective matches room perspective
[ ] Contact shadow present where it meets the floor
[ ] Shadows fall away from the stated light source
[ ] Upholstery and wood grain photographed, not generated
[ ] Colour verified against the physical piece
[ ] Dimensions stated in the copy regardless
```

**State dimensions in the copy regardless of the imagery.** No room scene substitutes for a measurement, and furniture returns are expensive.

## Don't

- **Don't generate patterned upholstery or specific wood grain.**
- **Don't omit scale reference objects.**
- **Don't leave ceiling height unstated.**
- **Don't leave camera height unstated.** You'll get a dramatic low angle.
- **Don't accept a floating piece.** Contact shadow required.
- **Don't accept contradictory shadows.** One dominant light source.
- **Don't rely on imagery for size.** Put dimensions in the copy.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-scale-reference`, `background-replacement-scenes`, `lifestyle-product-photography`, `multi-product-scene`, `product-image-qa-review`, `apparel-product-photography`, `ugc-lifestyle-photos`
