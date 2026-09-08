---
name: product-image-qa-review
description: Review generated product images before they ship, catching the artefacts and accuracy failures that pass casual inspection. Use whenever the user is about to publish product images, has a batch to check, or asks how to quality-check AI-generated product photography.
---

# Product Image QA

Generated product images fail in ways that are invisible at thumbnail size and obvious to a customer holding the item. A warped logo, an invented rear panel, a shifted colour — all pass a glance and none pass a return.

This is the review pass. It takes a few minutes per image and it's the difference between using generated imagery commercially and getting away with it until you don't.

## The two categories of failure

**Artefacts** — the image is wrong. Warping, garbling, halos, extra objects. Aesthetic, embarrassing, and usually easy to spot once you know where to look.

**Accuracy failures** — the image is *plausible but not your product*. Shifted colour, invented detail, fabricated text, a configuration you don't sell. Harder to spot and much more consequential: these drive returns, complaints and misrepresentation exposure.

**Accuracy is the one to prioritise.** A slightly soft edge costs nothing. A navy shirt rendered as charcoal costs a return, a refund and a review.

## The review pass

```
ACCURACY — against the real product, not the reference photo
[ ] Colour matches the physical product under neutral light
[ ] Shape and proportions correct
[ ] Label copy: correct, or invented?
[ ] Logo: exact shape, or re-drawn?
[ ] Materials and finish read correctly
[ ] Any face the references didn't cover — fabricated?
[ ] No features added that don't exist
[ ] No configuration or variant you don't sell
[ ] Nothing in frame that doesn't ship with it

ARTEFACTS
[ ] Zoom to 100%: edges clean, no halo
[ ] Check on white AND black for fringing
[ ] Text anywhere in frame: legible and correct
[ ] Hands, if present: correct fingers, plausible joints
[ ] Reflections: consistent with the scene
[ ] Shadow direction matches the light source
[ ] Shadow softness matches the light quality
[ ] No duplicated or stray objects
[ ] No waxy or over-sharpened texture

COMPLIANCE
[ ] Background exactly 255,255,255 if a listing main image
[ ] Product fill 85%+ if a main image
[ ] No text, badge, watermark or border on a main image
[ ] Correct ratio, resolution and colour space
[ ] AI disclosure applied where required

SET CONSISTENCY
[ ] Same look as its siblings
[ ] Same product scale and orientation
[ ] Grid review at thumbnail size
```

## The checks that catch the most

**Compare against the real product, not the reference photo.** The reference has its own grade. Judging colour against it propagates whatever shift the reference already had.

**Check on white and black.** A light halo is invisible against white and obvious against black. Any cutout or composite gets both.

**Shadow direction.** If the light comes from the left, the shadow falls right. A shadow falling toward the light source reads as wrong immediately even to people who can't say why — and it's the most common physics error in composited scenes.

**Grid review at thumbnail size.** Lay the set out as a grid and look. Individually-fine images reveal their inconsistency instantly at the size a customer actually sees them. See `product-photo-consistency`.

**Any face the references didn't cover.** This is the accuracy failure people miss. The model generates a plausible rear panel with invented text, invented ports, invented markings. It looks fine. It's a misrepresentation. See `camera-angle-variation`.

## Using `analyze_image` as a second pass

`analyze_image` costs 1 credit and it describes images, extracts text and answers questions about visual content. That makes it a genuinely useful second reader.

```
analyze_image(
  image="<generated image>",
  prompt="List every object visible in this image. Transcribe any "
         "text exactly as it appears. Describe the light direction "
         "and where shadows fall."
)
```

What it catches that you skim past:

- **Invented text.** It transcribes what's actually there, which reveals garbled or fabricated label copy
- **Objects you'd stopped noticing** — a stray prop, a duplicated item
- **Light and shadow inconsistency**, described plainly

It isn't a replacement for looking. It's a cheap second opinion on a batch, and it's particularly good at the text check.

## Where the accuracy line sits

Retouching a photograph and generating a representation are different acts, and the standard differs.

**Fine:**
- Removing dust, lint, fingerprints, sensor spots
- Removing a stray prop or a studio reflection
- Relighting and grading within accurate colour
- Repairing a cutout edge or a generation artefact
- Generating a new *scene* around an accurate product

**Not fine:**
- Removing a defect that ships with every unit
- Changing colour, shape, proportions or finish
- Generating a product face you can't verify
- Inventing label or regulatory copy
- Showing a configuration you don't sell
- Implying included accessories that aren't

**The test: does this image accurately represent what arrives in the box?** If not, it's a claim problem regardless of how good it looks.

For regulated categories — cosmetics, supplements, food, electrical, children's products — generated imagery goes through the same approval as photography, and required regulatory faces must be photographed. See `marketplace-image-compliance`.

## Batch QA

At catalogue scale, tier the review.

```
Tier 1 — every image
  Grid review at thumbnail. Catches consistency and obvious faults

Tier 2 — every main listing image
  Full checklist. These are the images customers rely on

Tier 3 — a random sample of secondary images
  Full checklist on 10-20%, plus anything the grid flagged
```

**Sample randomly, not the first ten.** The first ten are usually the ones you were watching while you built the pipeline, and therefore the least likely to be wrong.

Never auto-publish. A pipeline that ships without a human look will eventually ship a warped logo as a paid ad. See `product-photo-batch-pipeline`.

## Log the failures

Faults in generated imagery are systematic, not random. Logging them turns a QA pass into a prompt improvement.

```
Fault                        Cause                    Fix
label text garbled           FLUX on text             use gpt-image-2, or overlay
halo on cutout edge          transparent product      inpaint the edge
colour shifted warm          scene grade              neutral render for listings
rear panel invented          no rear reference        photograph it
shadow wrong direction       scene prompt vague       state light direction
waxy texture                 aggressive upscale       generate at 4K instead
```

One pass through that table usually fixes the next batch before it's generated.

## Don't

- **Don't judge colour against the reference photo.** Judge against the product.
- **Don't check only against white.** Check black too.
- **Don't publish a product face you can't verify.**
- **Don't accept invented label text.**
- **Don't judge images individually.** Grid review.
- **Don't sample the first ten.** Sample randomly.
- **Don't auto-publish.**
- **Don't treat a shadow falling the wrong way as acceptable.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`marketplace-image-compliance`, `product-photo-consistency`, `camera-angle-variation`, `inpainting-product-fixes`, `white-background-ecommerce`, `product-photo-batch-pipeline`, `ugc-photo-dump-format`
