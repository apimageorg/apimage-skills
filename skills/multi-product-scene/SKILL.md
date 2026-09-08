---
name: multi-product-scene
description: Compose several products in one image — range shots, bundles, collections — with correct relative scale and no invented items. Use whenever the user wants a range shot, a product family image, a bundle or kit image, or several SKUs in one scene.
---

# Multi-Product Scenes

Range shots do a job single-product images can't: they show the family, imply choice, and communicate relative size across variants. They're also the format where generation most reliably invents an extra product.

## The two failure modes

**Invented products.** Ask for "the range of four bottles" and you frequently get five, or four bottles where one is a shape you don't make. The model fills the composition rather than counting.

**Wrong relative scale.** Products generated independently and composited, or generated together without dimension anchors, end up at similar frame-fill regardless of their real sizes. A 30ml and a 200ml bottle rendered the same height actively misinforms.

Both are accuracy failures, not aesthetic ones.

## Composite, don't generate together

The reliable workflow for anything where accuracy matters.

```
1. Photograph or generate each product separately, accurately
2. remove_background per product        2 credits each
3. Composite in an editor at TRUE relative scale
4. replace_background or generate the scene behind    3 credits
5. Add consistent shadows per product
```

**Compositing gives you control over count and scale**, which is exactly what generating them together loses. It costs a few credits more and it's the difference between a range shot you can publish and one that misrepresents your line.

For a purely atmospheric scene where the products are secondary, generating together is acceptable — but count them.

## If generating together

```
generate_image(
  model="flux-2-pro",
  reference_images=[PROD_A, PROD_B, PROD_C],
  prompt="Exactly three bottles, no more and no fewer, standing in "
         "a row on pale oak. Left bottle 30ml and visibly smallest, "
         "centre bottle 100ml, right bottle 200ml and visibly "
         "tallest — correct relative proportions. Soft daylight "
         "from the left. Products unchanged — same labels, shapes "
         "and colours.",
  aspect_ratio="16:9",
  seed=8812
)
```

Three clauses doing the work:

**"Exactly three, no more and no fewer."** State the count explicitly and redundantly. It still sometimes gets it wrong — count the output.

**Named real volumes with relative descriptions.** "30ml and visibly smallest" anchors the proportion. Volumes alone don't; the model doesn't reliably know what 30ml looks like.

**"Products unchanged."** Reduces label and shape drift across three subjects, which is harder than one.

## Relative scale

The check that has to be done numerically, not by eye.

```
Real:      30ml bottle = 90mm tall
           200ml bottle = 170mm tall
           Ratio = 1 : 1.89

In image:  measure the pixel heights
           Ratio should be 1 : 1.89 (± a little for perspective)
```

**Measure it.** If the ratio in the image is 1:1.2, the small bottle looks bigger than it is and the customer choosing the 30ml gets a surprise. This is a specification claim, not a styling choice.

See `product-scale-reference`.

## Composition for a range

| Arrangement | Reads as | Good for |
|---|---|---|
| Straight row, aligned bases | Ordered, catalogue | Ranges, sizes |
| Ascending by size | The range, clearly | Multi-size lines |
| Loose cluster, varied heights | Editorial, lifestyle | Social, ads |
| Overhead flat lay | Complete, graphic | Kits, bundles |
| One hero forward, others behind | A featured product plus range | Launches |
| Grid, evenly spaced | Systematic, technical | Large ranges |

**Ascending by size is the clearest arrangement for a multi-size line** and it does the scale communication automatically.

**Align the bases** for a row shot. Products at slightly different heights on the surface read as sloppy.

## Bundles and kits

A specific case with a specific claim risk.

- **Show exactly what's included.** Nothing more
- **Nothing in frame that isn't in the box.** A styled prop in a bundle image reads as included
- **Count matters.** If the kit has four items, show four
- **Don't imply a size or quantity** the bundle doesn't contain

**The bundle image is a contents claim.** A customer who counts five items in the photo and receives four has a legitimate complaint. See `product-image-qa-review`.

## Consistency across the products

If the products are generated or relit separately and then composited, they have to share a look or the composite reads as a collage.

```python
LOOK = ("Soft daylight from the left at 45 degrees, gentle falloff "
        "to the right, neutral white balance, subtle contact shadow "
        "beneath.")
```

Same clause verbatim for each product, same seed. Then when composited, the light direction agrees across all of them — which is what makes a composite read as one photograph.

**Shadows must all fall the same way.** Three products with shadows in three directions is the tell. See `product-photo-consistency`.

## The QA pass

```
[ ] Count the products. Matches the intended count exactly
[ ] No invented product, variant or colourway
[ ] Relative scale measured against real dimensions
[ ] All shadows fall the same direction
[ ] All products share the same light and grade
[ ] Bases aligned, if a row shot
[ ] Nothing in frame that isn't included, for a bundle
[ ] Each label correct and undistorted
[ ] Colour of each verified against the real product
```

**Count them.** It's the check that sounds unnecessary and catches the most.

## Don't

- **Don't generate a range together** where accuracy matters. Composite.
- **Don't omit the explicit count.** And count the output anyway.
- **Don't rely on volumes alone** for scale. Add relative descriptions.
- **Don't eyeball relative scale.** Measure it.
- **Don't let shadows fall in different directions.**
- **Don't include props in a bundle image.**
- **Don't paraphrase the look clause** between products.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-scale-reference`, `flat-lay-composition`, `product-photo-consistency`, `packaging-mockup-generation`, `background-replacement-scenes`, `product-image-qa-review`
