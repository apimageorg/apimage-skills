---
name: ugc-selfie-style-photos
description: Generate UGC-style still images that read as a customer's phone photo rather than product photography. Use whenever the user needs UGC images, customer-style photos, social proof stills, review-section imagery, or phone-camera-look product shots.
---

# Selfie and Phone-Photo Style Stills

UGC-style stills are the cheapest high-performing creative there is: one image call at a few credits, no async job, no motion artefacts, no lip sync. They outperform studio product shots in social ads for the same reason UGC video does, and they're an order of magnitude cheaper to iterate on.

The difficulty is that image models are trained toward good photography, and you are asking for slightly bad photography on purpose.

## The tells of a phone photo

The whole skill is a list of imperfections to request explicitly.

| Signal | Prompt clause |
|---|---|
| Phone lens | "Shot on a phone camera, slight wide-angle distortion" |
| Uneven light | "Single window light from the left, no fill, one side in shadow" |
| Mixed colour | "Warm ceiling light mixing with cool daylight" |
| Casual framing | "Slightly off-centre, tilted a few degrees, product not centred" |
| Real depth | "Deep focus, background visible and in focus" |
| Real surroundings | "Ordinary kitchen counter with other items and some clutter" |
| Slight softness | "Very slight motion blur, not tack sharp" |
| Sensor noise | "Faint grain in the shadows" |
| No styling | "Nothing arranged, items where they were left" |
| A person's presence | "A hand partly in frame, holding it" |

And the negatives, which do as much work as the positives:

```
Not studio. Not commercial photography. No softbox, no reflector,
no seamless backdrop. Not styled, not arranged, not retouched.
```

**"Deep focus, background in focus" is the single most effective clause.** Shallow depth of field is the strongest signal of a real camera, and phones at normal distance don't produce it. Remove the bokeh and half the studio look goes with it.

## A working prompt

```
generate_image(
  model="flux-2-pro",
  prompt="A hand holding a small amber glass bottle over a wooden "
         "kitchen worktop. Shot on a phone camera, slight "
         "wide-angle distortion, held slightly too close. Single "
         "window light from the left, no fill, right side falling "
         "into shadow. Warm ceiling light mixing with cool "
         "daylight. Bottle slightly off-centre, frame tilted a few "
         "degrees. Deep focus, the kettle, a tea towel and some "
         "post arranged nowhere in particular, all visible in "
         "focus behind. Faint grain in the shadows, very slight "
         "motion blur. Not studio, not commercial photography, no "
         "softbox, not styled, not retouched.",
  reference_images=[PRODUCT_ASSET],
  aspect_ratio="4:5",
  seed=8812
)
```

`generate_image` costs 1-9 credits depending on model and settings, and returns synchronously — so this is a fast loop. Generate twenty, keep three. `enhance_prompt` is free if you want the model's expansion before spending anything.

## Aspect ratios that read as phone photos

- **4:5** — the native Instagram feed portrait. The default for this
- **9:16** — stories and Reels stills
- **3:4** — reads as an older phone photo, which is a useful signal
- **1:1** — safe but reads as more deliberate
- **16:9** — avoid. Landscape reads as professional

**A perfectly square, perfectly level composition is a studio signal.** Portrait and slightly tilted is the ordinary case.

## The product still has to be identifiable

The tension in this format: you want an imperfect photo of a perfectly accurate product.

- **Real product photography as the reference.** Always. See `product-photo-from-reference`
- **One clear, legible view of the label** in the set, even if others are casual
- **Never let "casual" become inaccurate.** A distorted label is a misrepresentation, not a stylistic choice
- **No generated printed text.** Same rule as everywhere else
- Run the QA pass on every keeper — the imperfect look makes it easier to overlook a genuine defect. See `product-image-qa-review`

**Shoot the set as a mix:** two or three casual, one clear. The clear one does the identification work; the casual ones do the persuasion.

## Hands and people

A hand in frame is the strongest single UGC signal in stills, and hands are the most common generation failure.

- **One hand, simple grip.** Not two hands interacting
- **Partly out of frame** is both more natural and safer — fewer fingers to get wrong
- **Check every finger at full size.** Zoom in. This fails often
- **A face makes it a person**, with all the persona and consistency questions that follow. Prefer hands. See `product-in-hand-shots`

If you do include a face, use a saved character asset so it's the same person across the set. Free. See `ugc-character-consistency`.

## Where these are used, and what changes

| Placement | Adjustment |
|---|---|
| Paid social | Strongest UGC signal. Deep focus, visible clutter |
| Product-page gallery, secondary | Cleaner. Real setting, but tidier |
| Review section | Most casual of all. Slightly bad is correct |
| Ad carousel | Vary framing across slides so it reads as several photos |
| Email | Slightly cleaner. Renders small |
| Marketplace listing | **Don't.** Compliance requires clean primary images |

**Marketplace primary images have hard requirements** — pure white backgrounds, product fills the frame, no props or text. UGC stills fail those and can get a listing suppressed. Keep them to secondary slots. See `marketplace-image-compliance` and `amazon-listing-images`.

## Don't fake customer photos

The line that governs the format.

- **Never present a generated image as a customer's own photo.** Review sections, "customer gallery", social proof modules — inserting generated images there fabricates evidence
- **Never generate a photo attributed to a named person**
- **Own channels and ads are fine.** The UGC *style* is a legitimate creative choice
- **Label per platform AI-content rules** where they apply
- If you want real customer photos, run a request campaign — it's cheap, and a real one plus a generated set is the strongest combination

**Style is fine. Attribution is not.** See `ugc-disclosure-compliance`.

## Don't

- **Don't accept shallow depth of field.** Deep focus.
- **Don't omit the negative clauses.**
- **Don't shoot landscape.**
- **Don't centre and level everything.**
- **Don't generate the product from a description.** Real reference.
- **Don't skip the finger check** at full size.
- **Don't use these as marketplace primary images.**
- **Don't place generated images in a customer-photo section.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-lifestyle-photos`, `product-in-hand-shots`, `ugc-authenticity-signals`, `social-commerce-product-images`, `product-photo-from-reference`, `ugc-photo-dump-format`
