---
name: apparel-product-photography
description: Generate apparel and fashion product images — ghost mannequin, flat lay, on-model — and handle fabric, fit and colour accuracy. Use whenever the user is photographing clothing, fashion, accessories, footwear, or asks about apparel listing images or on-model shots.
---

# Apparel Product Photography

Apparel has the highest return rate in ecommerce, and imagery drives most of it. A customer returns a garment because it didn't look like the photo — the colour was off, the fit read differently, the fabric looked heavier.

That makes accuracy the priority over aesthetics in a way it isn't for most categories.

## The four apparel shot types

| Type | What it is | Use for |
|---|---|---|
| **Ghost mannequin** | Garment holding its shape, form invisible | **Main listing image.** Marketplace-compliant |
| **Flat lay** | Garment laid flat, shot from above | Detail, styling, social |
| **On-model** | Worn by a person | Fit, drape, scale, desire |
| **Detail macro** | Fabric, stitching, hardware, label | Quality signals |

**Ghost mannequin is usually the compliant main image.** Several marketplaces prohibit a visible mannequin or hanger in the main apparel image, and ghost mannequin is the standard answer — the garment keeps its three-dimensional shape with no visible support.

## Ghost mannequin

```
1. Photograph on a mannequin or form
2. remove_background            2 credits → isolate the garment
3. edit_image inpainting                  → remove visible form at the
                                            neckline and openings
4. Composite on 255,255,255 white         → in an editor
5. Add a consistent soft contact shadow
```

The inpainting step is where it's won or lost. The neckline, cuffs and hem openings need to show the inside of the garment, not a mannequin edge.

```
edit_image(
  image=CUTOUT,
  mask=NECKLINE_REGION,
  prompt="The inside back of the collar continuing naturally, same "
         "fabric and colour, soft interior shadow. No mannequin, "
         "no visible form."
)
```

**Mask the openings, not the whole garment.** And describe what should be there — the interior of the garment — rather than what to remove. See `inpainting-product-fixes`.

## Fabric is the hard part

Generated fabric fails in specific, recognisable ways.

| Fabric | Problem | Prompt against it |
|---|---|---|
| Knit and ribbed | Pattern direction drifts | "consistent rib direction, vertical" |
| Denim | Weave and wash detail smears | "visible twill weave, defined wash" |
| Silk and satin | Sheen becomes plastic | "soft natural sheen, not glossy" |
| Lace and mesh | Pattern becomes mush | Photograph it. Don't generate |
| Print and pattern | **Motifs get reinvented** | Photograph it. Don't generate |
| Leather | Grain becomes uniform | "irregular natural grain" |
| Wool and tweed | Texture flattens | "visible fibre texture, matte" |

**Never generate a printed pattern.** The model reinvents the motifs — different flowers, different stripe spacing, a re-drawn logo. That's a misrepresentation of the actual garment, and printed apparel is exactly where customers notice.

For patterned, printed or logo'd garments: photograph them. Generate the *scene* around an accurate garment, not the garment itself. See `product-photo-from-reference`.

## Colour accuracy is the return driver

Apparel colour is the single largest source of image-driven returns.

- **Neutral white balance for the listing image.** Always. No creative grade
- **Check against the physical garment** under neutral light, not against the reference photo
- **A warm grade turns navy into charcoal**, black into brown, white into cream
- **Never generate a colourway you don't stock.** Recolouring is the fastest route to a returns problem
- **State it in the prompt:** "neutral white balance, accurate colour, no grade"

Keep graded lifestyle images separate from the listing image. See `white-background-ecommerce`.

## On-model shots

The most persuasive apparel image and the one with the most constraints.

**Rights.** Don't use a real person's likeness without permission — including stock portraits whose licence excludes AI or synthetic use. Generate a model and save them as a character brand asset so the whole catalogue uses the same person. See `ai-avatar-presenter`.

**Fit representation.** This is the accuracy line specific to apparel:

- **Don't generate a fit the garment doesn't have.** A loose shirt rendered as fitted is a misrepresentation
- **Don't reshape the model's body.** Several markets require disclosure of body-altering retouching in advertising, and it's a live regulatory area
- **Represent the size you're showing.** If the model is wearing a size S, don't imply it's how the garment fits everyone
- **Size and fit information belongs in the copy**, not implied by a generated drape

**Practical approach:** photograph the garment on a real model for fit-critical images, and use generation for scene, background and styling variants around that photograph. `replace_background` on a real on-model shot gives you ten settings for 3 credits each with the fit intact.

## Footwear and accessories

- **Footwear** needs a consistent angle convention across the catalogue — usually a three-quarter with the toe toward camera. Sole shots and detail shots as secondaries
- **Bags** need a scale reference. A bag photographed alone reads as any size. See `product-scale-reference`
- **Jewellery and watches** are reflective and get their own treatment. See `jewelry-reflective-products`
- **Hardware detail** — zips, buckles, clasps — are quality signals worth a macro shot. See `product-detail-macro`

## The catalogue spec for apparel

```
Main image        ghost mannequin, pure white, 85%+ fill, 1:1
Angle convention  front, back, side, detail, on-model
Colour            neutral white balance, verified against the garment
Fabric texture    photographed, not generated, for anything patterned
Shadow            soft contact, identical across the catalogue
Model             same generated character, or real with a release
Fit               represented accurately, size stated in copy
Resolution        4K master, downscaled per channel
```

See `product-photo-consistency`.

## Don't

- **Don't generate printed patterns or logos.** Photograph them.
- **Don't leave a visible mannequin or hanger** in a main listing image.
- **Don't use a graded render** as the apparel listing image. Colour drives returns.
- **Don't recolour into a colourway you don't stock.**
- **Don't generate a fit the garment doesn't have.**
- **Don't reshape a model's body.**
- **Don't use a stock portrait** as a model reference without checking the licence.
- **Don't skip the scale reference** on bags and accessories.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`model-wearing-product`, `flat-lay-composition`, `product-detail-macro`, `white-background-ecommerce`, `product-image-qa-review`, `jewelry-reflective-products`, `ugc-get-ready-with-me`
