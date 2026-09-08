---
name: ugc-photo-dump-format
description: Build photo-dump and carousel sets that read as a handful of casual phone photos rather than a designed ad. Use whenever the user needs a carousel, a multi-image ad set, photo-dump content, or several images that must feel like one person's camera roll.
---

# Photo Dump and Carousel Sets

A photo dump is a deliberately unpolished set of images posted together. It performs because it reads as someone's camera roll, and because carousels get more dwell time than single images on most platforms.

The craft is in the **variation between images**. A set where every shot has the same framing, light and distance reads as a shoot, not a roll — which is the one thing that has to not happen.

## What a real camera roll looks like

| Real roll | Designed set |
|---|---|
| Varied distances, some too close | Consistent framing |
| Some shots slightly bad | All usable |
| Inconsistent light across images | One lighting setup |
| Different times of day | One session |
| A few unrelated shots | All on-message |
| Different angles, some awkward | Considered angles |
| One or two near-duplicates | No repetition |

**Deliberately include one weak image.** A slightly blurred shot, an awkward crop, a photo of something incidental. It's the strongest signal in the whole format, and it's the thing brand teams remove.

## Build the set with a spread

Six to nine images. Assign each a job before generating.

```
1  Anchor        the product clearly. Legible label. The one clean shot
2  In use        a hand, mid-action, slightly blurred
3  Wide context  the whole room, product small in frame
4  Too close     macro, awkwardly tight, partly out of frame
5  Result        what changed, casual framing
6  Incidental    the mug, the window, the dog. Product absent
7  Duplicate-ish near-identical to 2, slightly different angle
8  Text shot     a screenshot or note. Optional
```

**The incidental image is what makes it a roll.** A set where every photo contains the product is an ad carousel wearing a photo dump.

**The anchor image is what makes it sell.** One clear, legible, accurate view is non-negotiable — everything else can be casual.

## Vary the generation, deliberately

The technical point: consistency is normally the goal, and here it isn't. So vary the seed and the prompt on purpose.

```python
PROD = "<product asset>"
BASE = ("Shot on a phone camera, deep focus with the background "
        "in focus, no styling, nothing arranged. Not studio, not "
        "commercial photography, no softbox, not retouched.")

SHOTS = [
  # anchor - clean, seed A
  ("The bottle standing on a wooden worktop, front label facing "
   "camera, clearly legible. Soft window light from the left. " + BASE,
   "4:5", 8812),

  # in use - motion, different seed
  ("A hand tipping the bottle onto a cloth, slight motion blur, "
   "frame tilted, subject slightly off-centre. Warm ceiling light "
   "mixing with daylight. " + BASE, "4:5", 3311),

  # wide - product small
  ("An ordinary rented-flat kitchen at midday, the bottle small on "
   "the worktop among a kettle, post and a mug. Flat bright "
   "daylight, slightly blown out near the window. " + BASE,
   "4:5", 5502),

  # too close
  ("Extreme close-up on the bottle neck and cap, held too close to "
   "the lens, part of it cut off by the frame edge, slightly out "
   "of focus. " + BASE, "4:5", 7710),

  # incidental - no product
  ("A mug of tea on a windowsill in the late afternoon, low warm "
   "sun, nothing else in frame. " + BASE, "4:5", 9104),
]
```

**Different seed per image is the whole trick.** A fixed seed gives one visual treatment across the set, which is exactly the studio-shoot signal you're avoiding. Vary the light description too — real rolls span hours.

Keep the aspect ratio consistent though. Mixed ratios in a carousel crop badly and look like an error rather than a choice.

## Order matters more than in video

Carousels are judged on the first image and abandoned on the second.

```
Slide 1   the hook. Strongest, most legible, most curious
Slide 2   the payoff or the in-use shot. Earns slide 3
Slide 3+  the roll. Context, close-ups, incidentals
Last      the quiet CTA, or nothing
```

**Slide 1 is a thumbnail.** It's judged at postage-stamp size in a feed, so test it small and muted. If the product isn't identifiable at that size, reorder.

**Don't put the weak image at position 1 or 2.** Bury it at 4 or 5, where it does its authenticity work without costing you the swipe.

## Accuracy, in one place

The relaxed aesthetic doesn't relax the rules.

```
[ ] Real product photography as the reference for every product shot
[ ] The anchor image has a fully legible, undistorted label
[ ] No generated printed text or logos anywhere
[ ] Colour consistent across the set — a different colour per image
    reads as a different product
[ ] Nothing shown that isn't included
[ ] Fingers checked at full size on every hand shot
```

**Colour drift across a varied set is the specific risk here**, because varying the light and the seed also varies the rendered colour. Check the set laid out together, not image by image. See `product-image-qa-review` and `product-photo-consistency`.

## Where it works

- **Paid social carousels.** Strong. Test against a single-image ad
- **Organic feed posts.** Native to the format
- **Product-page secondary gallery.** Good, after the clean shots
- **Email.** Only the anchor. Rolls don't render
- **Marketplace listings.** Secondary only. Primary images have hard clean-image requirements. See `marketplace-image-compliance`

And the boundary: **never place a generated set in a customer-photo or review gallery.** UGC style is a creative choice; attributing generated images to customers fabricates evidence. See `ugc-disclosure-compliance`.

## Don't

- **Don't shoot the set at one distance** or one light.
- **Don't use one seed** across the set.
- **Don't mix aspect ratios.**
- **Don't put the product in every image.**
- **Don't remove the weak image.** Move it to position 4.
- **Don't lead with the weak image.**
- **Don't let colour drift** across the set.
- **Don't file generated images as customer photos.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-selfie-style-photos`, `ugc-lifestyle-photos`, `product-photo-consistency`, `social-commerce-product-images`, `product-image-qa-review`, `ugc-authenticity-signals`
