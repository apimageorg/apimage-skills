---
name: before-after-transformation-video
description: Generate before-and-after transformation video that is persuasive without making claims the product cannot support. Use whenever the user wants a transformation clip, before-and-after content, a cleaning or beauty or repair demo, or a satisfying-result video.
---

# Before-and-After Transformation

Transformation is the highest-retention format in short-form. The viewer sees a problem state, knows a resolution is coming, and stays for it. That's a complete narrative in six seconds.

It's also the format most likely to produce a false claim, because the model will happily generate a result the product doesn't deliver.

## Why it holds attention

The format works because it front-loads a promise. The "before" frame is the hook — it establishes stakes and implies the payoff without any copy at all. Nothing else in short-form does that as efficiently.

Which means the **before state is the more important half**, and it's the half people rush.

## Generate as two beats, not one

Asking one generation to morph a dirty surface into a clean one produces the characteristic melting transition that reads as AI immediately.

```
Beat 1  "before"  3-4s   the problem state, static or slow motion
Beat 2  "action"  4-5s   the product being applied
Beat 3  "after"   3-4s   the result state
```

Three separate generations, cut together. The cut does the transformation, which is both more convincing and more controllable than a generated morph.

**Match the framing exactly across beats.** Same camera position, same distance, same angle, same light. That's what makes the comparison legible — a before and after from different angles proves nothing and looks evasive.

```
FRAMING = ("Fixed camera, 45 degrees above, subject centred, "
           "soft window light from the left, same distance.")

beat_before = f"The stained worktop, close-up. Nothing moves. {FRAMING}"
beat_action = f"Cloth wipes across the worktop, mid-motion. {FRAMING}"
beat_after  = f"The clean worktop, light catches the surface. {FRAMING}"
```

Same seed across all three. See `multi-scene-video-assembly` and `seed-locked-iteration`.

## The before state has to be believable

Two failure modes, opposite directions.

**Too clean.** A "before" that isn't actually bad makes the after unimpressive. The transformation has nowhere to travel.

**Too staged.** A comically extreme before reads as fake and the whole clip loses credibility. Viewers are well-calibrated on this — they've seen a lot of ads.

The target is **a genuinely realistic bad state**: the mess that actually accumulates, the wear that actually happens, the room that actually looks like that on a Tuesday.

```
Weak:   "a slightly dusty shelf"
Weak:   "an absurdly filthy kitchen, thick grime everywhere"
Right:  "a worktop with coffee rings, a few crumbs, a dried splash
         near the tap. Ordinary domestic mess, realistic."
```

## Use a real before, where you can

The most credible version uses actual photography of the actual before state, animated with image-to-video. Then only the after is generated.

```
1. Real photo of the before state
2. generate_video(mode="image-to-video", reference_images=[real_before])
   → animate the before
3. edit_image on the real before → the after state, same framing
4. generate_video(mode="image-to-video", reference_images=[after_still])
   → animate the after
```

`edit_image` is the right tool for step 3 — it supports inpainting and erasing via masked regions, which means you can remove the stain from the real photograph rather than generating a new scene. Same surface, same light, same everything, minus the problem.

That's a much stronger asset than two independently generated scenes, and it's cheaper.

## Where the claim line sits

This is the part that matters legally, and generated video makes it easy to cross without noticing.

**Not acceptable:**
- A result the product doesn't produce
- A timescale the product doesn't achieve — instant when it takes an hour
- A degree of change beyond what's typical
- A before state exaggerated to inflate the apparent effect
- Any of the above in health, beauty, weight loss, supplements or dental, where before-and-after imagery is **specifically regulated in most markets** and sometimes prohibited outright

**Acceptable:**
- A typical result, achieved as the product actually achieves it
- Time compression that is disclosed ("after 4 weeks")
- A representative before state
- Clear labelling that the visual is illustrative and AI-generated

**The test:** could you produce this result with the product, on camera, in the stated conditions? If not, the clip is a false claim regardless of how it was made.

For regulated categories, generated before-and-afters go through the same legal approval as filmed ones — and in several markets, synthetic before-and-after imagery for cosmetic and medical claims is not permitted at all. Check before generating, not after. See `video-brand-safety-moderation`.

## The satisfying-result craft

Retention on this format comes from the after beat landing well.

- **Hold the after slightly longer** than feels necessary. The payoff needs a beat to register
- **Add a small motion in the after** — light moving across the clean surface, steam, a hand withdrawing. A static after feels like a photo
- **Same light as the before.** Brightening the after is a cheat viewers notice
- **Don't cut away immediately.** The satisfaction is the product

Sound matters here more than most formats — the wipe, the click, the pour. Add it in the edit; don't try to generate it. See `music-audio-pairing`.

## Formats beyond cleaning

The structure generalises well:

| Category | Before | After |
|---|---|---|
| Cleaning | The mess | Clean |
| Repair | Broken, worn | Fixed |
| Organisation | Cluttered | Ordered |
| Beauty | Bare, undone | Done — **regulated** |
| Food | Raw ingredients | Finished dish |
| Software | Spreadsheet chaos | The dashboard |
| Home | Empty or dated room | Furnished, updated |
| Apparel | Ill-fitting or plain | Styled |

The software one is under-used and works: a genuinely messy spreadsheet, then the interface. Same structure, no regulatory exposure.

## Cost

```
1. Real before photo             free (yours)
2. edit_image → after still      generation credits
3. 3 draft generations           low
4. enhance the keepers           metered
```

Three short beats drafted then enhanced costs materially less than one long generated morph, and looks better. See `draft-then-enhance-workflow`.

## Don't

- **Don't generate the transformation as one morph.** Cut between beats.
- **Don't change framing between before and after.**
- **Don't exaggerate the before** to inflate the effect.
- **Don't brighten the after.** Same light.
- **Don't show a result the product doesn't produce.**
- **Don't generate before-and-afters in regulated categories** without legal sign-off — some markets prohibit synthetic ones entirely.
- **Don't cut away from the after too fast.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-demo-video`, `multi-scene-video-assembly`, `video-brand-safety-moderation`, `image-to-video-animation`, `video-hook-first-3-seconds`, `ugc-problem-solution-format`
