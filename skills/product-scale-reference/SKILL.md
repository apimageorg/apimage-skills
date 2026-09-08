---
name: product-scale-reference
description: Communicate product size in imagery using familiar reference objects, hands and dimension overlays, to cut size-driven returns. Use whenever a product's size is unclear from a photo, the user has size-related returns or questions, or is photographing anything where scale isn't obvious.
---

# Communicating Scale

A product photographed alone on white has no size. A candle could be 5cm or 30cm. A bag could be a clutch or a weekender. A planter could sit on a desk or on a patio.

Size confusion is one of the largest drivers of ecommerce returns, and it's almost entirely an imagery problem. One scale image in the gallery fixes it.

## Which products need it

| High need | Low need |
|---|---|
| Jewellery, watches | Furniture in a room shot |
| Bags, cases, luggage | Clothing with a size label |
| Candles, planters, vases | Standardised items (A4 paper) |
| Tools, hardware | Products with universal form |
| Electronics and accessories | — |
| Homeware and decor | — |
| Anything sold in several sizes | — |

**Anything sold in multiple sizes needs a scale image per size**, or at minimum a comparison image showing them together. A customer choosing between "medium" and "large" with no scale cue is guessing.

## The methods, ranked

**1. In hand.** The most intuitive and most universal. Everyone knows how big a hand is.

**2. Familiar object alongside.** A coin, a phone, a standard mug, a plug socket, an A4 sheet. Works well and reads instantly.

**3. In situ.** On a desk, on a shelf, worn, in a room. Communicates size and use simultaneously.

**4. Dimension overlay.** Measurements drawn on the image. Precise, unambiguous, and the least appealing — but the right choice for technical products.

**5. Comparison shot.** All size variants together. Essential for multi-size ranges.

**Use two of these, not one.** A hand shot plus a dimension overlay covers both intuition and precision, and different customers rely on different cues.

## Generating a hand-held shot

```
generate_image(
  model="flux-2-pro",
  reference_images=[PRODUCT_REFS],
  prompt="The same candle held in an adult hand, fingers wrapped "
         "around it naturally, hand partially cropped at the wrist. "
         "Plain neutral background, soft even light. Product "
         "unchanged — same proportions, label and colour. Hand at "
         "natural adult scale.",
  aspect_ratio="1:1",
  seed=8812
)
```

**"Hand at natural adult scale"** is worth stating, because the model will otherwise scale the hand to the product rather than the product to the hand — which defeats the purpose entirely and, worse, misinforms.

Hands are hard to generate. Mitigations:

- **Crop at the wrist**, so there's less hand to get wrong
- **Fingers wrapped, not splayed** — fewer visible digits, fewer errors
- **Partially out of frame** where possible
- **Check the finger count and joint plausibility** every time

For a critical image, photograph the hand shot. It's one photo and it removes the whole risk. See `product-in-hand-shots`.

## Familiar-object references that work

```
UK/EU coin          small items, jewellery
Standard phone      electronics, wallets, small goods
Standard mug        homeware, candles, small planters
Plug socket         furniture, wall-mounted items
A4 sheet            flat goods, prints, documents
Standard doorway    furniture, large items
Tennis ball         mid-size objects
Adult hand          almost anything
```

**Name the real dimension in the prompt.** "A 40cm side table beside it" anchors the scene. "A side table beside it" lets the model pick a size, which produces a scene that's internally consistent and externally wrong.

Caution: coins and paper sizes differ by market. For an international catalogue, a hand or a phone travels better than a specific coin.

## Dimension overlays

Add these in an editor, never in the generation — models render text and precise lines badly.

```
1. Generate or photograph a clean product image
2. Overlay dimension lines and measurements in an editor
3. Use both metric and imperial for international listings
4. Keep the styling identical across the catalogue
```

Best practice for the overlay itself:

- Thin lines, high contrast against the background
- Measurements outside the product silhouette, not on top of it
- Both unit systems for international selling
- A consistent style — same font, weight, line thickness across every product

**Never let a generation produce the numbers.** An invented dimension is a specification error with returns attached. Real measurements, overlaid.

## Multi-size ranges

For a product sold in several sizes, a comparison image is the highest-value gallery slot you have.

```
1. Photograph or generate each size accurately
2. Composite them side by side at true relative scale
3. Label each with its dimensions
4. Include a familiar object for absolute scale
```

**True relative scale is the point.** Generating the three sizes independently produces three images at similar frame-fill, which makes them all look the same size. Composite them at their real proportions.

## The claim line

Scale imagery is a specification claim.

- **Never let a generated scale reference misrepresent size.** A hand generated too small makes the product look bigger, and that's the return
- **Verify the relative scale against real measurements** before publishing
- **State dimensions in the copy regardless.** Imagery supports it; the copy is the specification
- **Don't imply included accessories** through a scale prop that isn't shipped

See `product-image-qa-review`.

## The QA pass

```
[ ] The reference object is at correct real-world scale
[ ] Product-to-reference proportion matches real dimensions
[ ] Hands, if present: plausible, correct digits, adult scale
[ ] Multi-size comparisons at true relative scale
[ ] Overlay measurements match the real specification
[ ] Both metric and imperial for international listings
[ ] Overlay styling consistent across the catalogue
[ ] Dimensions also present in the copy
```

**Measure the proportion in the image.** If the product should be 1.5× the width of a phone, check that it is. This is a measurable check, not a judgement.

## Don't

- **Don't publish a product with no scale cue** where size isn't obvious.
- **Don't let the model scale the reference to the product.** State the real dimension.
- **Don't generate dimension numbers.** Overlay real ones.
- **Don't generate multi-size comparisons independently.** Composite at true scale.
- **Don't use a market-specific coin** for an international catalogue.
- **Don't rely on imagery alone.** Dimensions in the copy.
- **Don't use a scale prop you don't ship** without making that clear.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-in-hand-shots`, `furniture-room-scenes`, `jewelry-reflective-products`, `product-detail-macro`, `product-image-qa-review`, `marketplace-image-compliance`, `ugc-haul-format`
