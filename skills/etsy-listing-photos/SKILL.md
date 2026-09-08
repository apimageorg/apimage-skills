---
name: etsy-listing-photos
description: Produce Etsy listing photos, where handmade authenticity matters and over-polished generated imagery actively hurts. Use whenever the user sells on Etsy or a handmade or craft marketplace, or asks how product imagery should differ there.
---

# Etsy Listing Photos

Etsy is the marketplace where generated imagery is most likely to backfire. Buyers are there specifically because they want something made by a person, and the visual signals of mass production — seamless studio lighting, perfect symmetry, flawless surfaces — work against you.

The polish that helps on Amazon hurts here.

## What Etsy buyers respond to

| Reads as handmade | Reads as mass-produced |
|---|---|
| Natural daylight, visible falloff | Even studio lighting |
| A real surface — wood, linen, stone | Seamless white sweep |
| Slight asymmetry and irregularity | Perfect symmetry |
| Visible maker's marks and tool traces | Flawless machine finish |
| A workshop or home setting | Studio |
| A hand in frame | Product isolated |
| Natural, imperfect styling | Art-directed styling |

**Visible irregularity is a feature, not a fault.** A slightly uneven glaze, a visible throwing ring, a hand-stitched seam that isn't machine-perfect — these are the evidence of handmade, and generated imagery smooths them away by default.

Prompt against it explicitly:

```
"Natural daylight from a window, visible slight irregularity in the
 glaze, a faint throwing ring on the base, real wooden workbench
 surface with visible grain and use marks. Handmade, not
 mass-produced. Not studio, not commercial product photography."
```

The negative framing at the end matters. The model's prior is strongly toward commercial polish. See `lifestyle-product-photography`.

## The honest constraint

Etsy sells handmade, vintage and craft goods, and its policies require accurate representation of what a buyer receives — including that handmade items are genuinely handmade.

**That creates a specific tension with generated imagery:**

- **A generated image of a handmade item is not a photograph of the item the buyer receives.** Handmade items vary between units, and that variation is part of what's being sold
- **Buyers reasonably expect the photo to show the actual or a representative item**
- **Generated imagery that smooths away variation** misrepresents what handmade means

**The defensible position:** photograph the actual item. Use generation for the *scene, background and light* around a photograph of the real piece.

```
1. Photograph the actual item                 the representation
2. remove_background            2 credits
3. replace_background           3 credits     → the scene, relit
4. Verify the item is unchanged
```

That gives you five settings for a piece for 3 credits each, with the actual item — including its actual irregularities — intact. It's cheap, it's honest, and it produces better imagery than a studio sweep would.

Generating the *item* is where you cross into misrepresentation. Check Etsy's current policy on AI-generated imagery before publishing, and disclose where required. See `marketplace-image-compliance`.

## The Etsy gallery

Etsy allows several images, and the sequence differs from a general marketplace.

| Position | Image |
|---|---|
| 1 | Hero — the item, natural light, real surface. **Not white sweep** |
| 2 | Scale — in hand, or with a familiar object |
| 3 | Detail — the handmade evidence. Texture, marks, stitching |
| 4 | In use / in context |
| 5 | Alternative angle |
| 6 | Variations, if offered |
| 7 | Packaging, if it's part of the gift experience |
| 8 | The maker or workshop, if relevant |

**Position 3 is the most important and most under-used.** The detail shot showing tool marks, glaze variation or hand-stitching is the proof of handmade, and it's what converts a browser who's deciding between you and a factory listing.

**Position 8 — the maker or workshop — is genuinely persuasive on Etsy** in a way it isn't elsewhere. Buyers want to know who made it.

## Scale is unusually important

Etsy inventory skews small — jewellery, ceramics, prints, accessories — and listings routinely omit scale.

- **A hand or a familiar object in at least one image.** Non-negotiable for small items
- **Dimensions in the copy**, always
- **A comparison shot** for multi-size offerings

See `product-scale-reference` and `product-in-hand-shots`.

## Surfaces and props

The prop and surface vocabulary that works here:

```
Surfaces:  raw timber, worn workbench, linen, stone, unglazed ceramic,
           handmade paper, weathered wood
Props:     dried flowers, twine, brown paper, a hand, tools of the craft,
           other pieces from the range
Light:     window light, overcast daylight, morning side light
Avoid:     seamless white sweep, coloured backdrop paper, studio
           softbox look, styled marble, gold accents
```

**Tools of the craft as props** are strong — a potter's rib, an embroidery hoop, a jeweller's saw. They signal the process rather than just the product.

**One or two props maximum.** Etsy's aesthetic rewards restraint, and cluttered flat lays read as stock photography. See `flat-lay-composition`.

## Technical requirements

Lighter than Amazon's, and still worth meeting.

- **2000px on the shortest side** for Etsy's zoom
- **Landscape or square** works best in the search grid — check the current recommendation
- **sRGB**
- **No text or watermarks** on the primary image
- **Consistent across your shop.** Your shop page is a grid, same as a collection page. See `product-photo-consistency`

## The QA pass

```
[ ] The actual item is what's shown, not a generated approximation
[ ] Handmade irregularity visible, not smoothed away
[ ] Natural light with visible direction and falloff
[ ] Real surface, not a studio sweep
[ ] Detail shot showing maker's evidence present
[ ] Scale cue present
[ ] Colour verified against the physical item
[ ] Props restrained — two maximum
[ ] Shop grid review at thumbnail size
[ ] AI disclosure applied where required
```

## Don't

- **Don't generate the item.** Photograph it; generate the scene.
- **Don't use a white studio sweep.** It reads as mass-produced.
- **Don't smooth away irregularity.** It's the selling point.
- **Don't use even studio lighting.** Natural, directional, with falloff.
- **Don't over-style.** Two props maximum.
- **Don't omit the detail shot.** It's the handmade proof.
- **Don't omit scale.** Etsy inventory is small and buyers guess.
- **Don't skip the AI disclosure** where the policy requires it.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lifestyle-product-photography`, `product-detail-macro`, `product-scale-reference`, `flat-lay-composition`, `shopify-product-images`, `marketplace-image-compliance`
