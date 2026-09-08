---
name: ugc-get-ready-with-me
description: Produce GRWM and routine-sequence video where a product appears at its natural step in an application order. Use whenever the user wants get-ready-with-me content, beauty or skincare routine video, morning-routine creative, or multi-product sequence content.
---

# Get Ready With Me

GRWM is the dominant beauty and personal-care format, and its mechanism is *order*: the viewer learns where a product sits in a sequence they already have. That's a genuinely useful thing to communicate and it's why the format sells consumables so well.

The production challenge is that it's a long multi-beat sequence on one face, which is the hardest thing to keep consistent in generated video.

## Structure: the sequence is the content

```
0-3s     THE OCCASION    where they're going. Specific, and quick
3-6s     STEP ONE        the base. Product visible in hand
6-9s     STEP TWO
9-12s    STEP THREE      the hero product. Slightly more time
12-15s   STEP FOUR
15-17s   THE RESULT      same framing as the start
17-18s   ONE ACTION
```

**The occasion beat matters more than it looks.** "Work" and "a wedding" are different routines, and naming it is what makes the sequence feel like a real person's rather than a product list.

**Give the hero product one extra beat**, not five. A GRWM where one product gets half the runtime is an ad wearing a routine.

## The consistency problem, stated plainly

Six beats, one face, close framing, hands near the face, and the face is *changing on purpose*. That's every difficulty in generated video at once.

What actually helps:

- **A large reference set.** Five or six views, not one. Video models accept 9-30 reference images depending on model — use them. See `ugc-character-consistency`
- **Fixed seed** across every beat, for treatment consistency
- **Short beats.** 3-4 seconds each. Identity holds much better over 4 seconds than 15
- **Front and three-quarter only.** No hard profiles
- **One hand where possible.** Two hands near a face is the worst case for artefacts
- **Same bathroom.** Save it as a background asset. Free
- **State the wardrobe** in every prompt

```python
CREATOR = ["<front>", "<3/4 left>", "<3/4 right>", "<slight smile>",
           "<looking down>", "<neutral close>"]
BG      = "<bathroom asset>"
LOOK    = ("Handheld phone footage propped on a shelf, slight "
           "shake, slightly uneven exposure. Soft natural light "
           "from a window to the left, no fill, no ring light. "
           "Small ordinary bathroom, a few bottles visible on the "
           "side. Plain grey t-shirt. Unpolished, shot on a "
           "phone. Not studio, not commercial.")
SEED    = 4271

STEPS = [
  ("She smooths the cream over her cheek with two fingers, "
   "looking slightly off camera, not at the lens.", 4),
  ("She presses the serum in with her fingertips, quick and "
   "practical, already moving on.", 4),
]
```

**"Not looking at the lens" and "already moving on"** are what separate a routine from a demonstration. A GRWM presenter is doing a thing, not presenting it.

## Show the application, not the product

The commonest failure: each step becomes a product hold-up.

```
Wrong: she holds the bottle up, label to camera, and describes it
Right: she uses it in two seconds, label glimpsed as it passes
```

One clear legible second of the hero product somewhere in the video is enough for recognition. Every other appearance can be incidental. See `ugc-authenticity-signals`.

## Real skin is the whole credibility question

This format sits directly on top of the "misleading depiction" problem, because the result *is* the claim.

- **Generate visible real skin.** Texture, pores, unevenness. "Not airbrushed, not retouched, visible skin texture" in the prompt
- **Don't generate an improvement the product doesn't deliver.** A generated after-shot showing lines gone is a performance claim you have to substantiate — and can't
- **Age-appropriate to the claim.** A product addressing lines demonstrated on skin with none is unpersuasive and arguably misleading
- **Don't use generation as a retouching route** around advertising rules on beauty claims. Several markets specifically police exaggerated before-and-afters in this category
- **Cosmetic claim rules are strict**, and drift into medical territory ("repairs", "heals", "reduces wrinkles") moves you into a different regulatory regime

**The safe version of the result beat is "makeup applied", not "skin transformed."** Showing a routine completed is not a claim. Showing skin changed is.

See `ugc-disclosure-compliance` and `video-brand-safety-moderation`.

## The identity question

GRWM is first-person by construction — *my* routine. A generated presenter has no routine.

Workable framings:

| Framing | Notes |
|---|---|
| **Disclosed presenter demonstrating an order of application** | The clean version. "Here's the order these go in" |
| **Hands only, no face** | No identity at all. Works better than expected |
| A real creator's footage, generated b-roll around it | Needs a usage licence |
| Generated person: "this is my routine" | Fabricated experience |

**Hands-only GRWM is underrated.** The sequence and the order are the content; the face is optional. It removes the consistency problem, the lip sync cost and the disclosure complexity in one move. See `faceless-video-automation`.

## Don't

- **Don't build long beats.** 3-4 seconds each.
- **Don't use one reference image** for a six-beat face sequence.
- **Don't hold products up to camera** at every step.
- **Don't give the hero product half the runtime.**
- **Don't generate flawless skin.** Texture, explicitly.
- **Don't generate a result the product can't produce.**
- **Don't drift into medical language.**
- **Don't let a generated presenter call it their routine.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-day-in-the-life`, `ugc-character-consistency`, `ugc-authenticity-signals`, `multi-scene-video-assembly`, `faceless-video-automation`, `ugc-disclosure-compliance`
