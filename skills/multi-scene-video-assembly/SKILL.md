---
name: multi-scene-video-assembly
description: Build longer video from several short generated clips instead of asking one generation for a whole narrative. Use whenever the user wants a video longer than about 10 seconds, a multi-shot sequence, a video with several beats, or is getting morphing and drift from long single generations.
---

# Multi-Scene Assembly

The instinct for a 20-second video is to generate a 20-second clip. It's the wrong move on every axis: it costs more, it drifts and morphs, it holds subject identity worse, and you can't fix one bad moment without re-rendering all of it.

**Generate beats. Assemble in an editor.** That's how the output stops looking generated.

## Why short clips win

| | One 20s generation | Four 5s clips, cut |
|---|---|---|
| Subject identity | Degrades over the clip | Holds within each |
| Morphing and drift | Common | Rare |
| Cost of one bad moment | Re-render everything | Re-render 5 seconds |
| Iteration | Whole clip | Per beat |
| Pacing control | Whatever the model does | Yours, in the edit |
| Looks generated | Yes, usually | Much less |

The last row is the one that matters commercially. Long single generations have a characteristic drifting quality that reads as AI immediately. Cuts read as editing.

## Storyboard first

Decide the beats before generating anything. One motion per clip.

```
Beat 1  0-3s    HOOK        hands already mid-pour, close, fast
Beat 2  3-7s    PROBLEM     the mess / the before state
Beat 3  7-12s   PRODUCT     product in use, clear demonstration
Beat 4  12-16s  RESULT      the after state, satisfying
Beat 5  16-18s  CTA         product held to camera, room for text
```

Each beat is a separate generation with **one motion**. A beat that needs two motions is two beats. See `text-to-video-prompting`.

## Consistency across the beats

This is what separates an assembly from a collection of unrelated clips. Four things, and you want all of them:

**1. Brand assets for subject and setting.** Character, product and background saved once, referenced in every beat. This is the load-bearing one.

```
create_brand_asset(type="character", ...)
create_brand_asset(type="product", ...)
create_brand_asset(type="background", ...)
```

See `character-consistency-video`.

**2. A locked seed per beat family.** Same seed across beats in the same location gives a consistent grade and rendering character. See `seed-locked-iteration`.

**3. Identical look language in every prompt.** Same lighting description, same lens character, same grade words, verbatim. Copy-paste the look clause rather than rewriting it — a paraphrase produces a different look.

```
LOOK = ("Soft directional window light from the left, shallow depth of "
        "field, warm neutral grade, handheld with subtle shake.")

prompt = f"Hands already mid-pour, close-up. {LOOK} Camera locked."
```

**4. Same aspect ratio and resolution.** Obvious, and still a common mistake in a batch assembled from different sessions.

## Generating the set

```python
BEATS = [
    ("hook",    "Hands already mid-pour, no lead-in, close-up.", 3),
    ("problem", "Slow pan across the stained surface.",          4),
    ("product", "Product applied to the surface, wiping motion.", 5),
    ("result",  "Clean surface, light catches the shine.",        4),
    ("cta",     "Product held to camera, lower frame kept clear.", 2),
]

jobs = {}
for name, action, dur in BEATS:
    jobs[name] = generate_video(
        mode="image-to-video",
        model="flux-3-video-draft",
        reference_images=[PRODUCT_ASSET, BG_ASSET],
        prompt=f"{action} {LOOK} Everything else stays still.",
        aspect_ratio="9:16",
        resolution="hd",
        duration=dur,
        seed=4271,
    )
```

Draft every beat, judge them, then `enhance_video_draft` only the ones you're keeping. A beat that doesn't work costs one short draft to redo. See `draft-then-enhance-workflow`.

**Submit in waves** — there's a concurrent in-flight cap on video. Three at a time is a safe start. See `async-video-job-orchestration`.

## Cutting it

- **Cut on motion.** A cut that lands mid-movement is invisible; a cut between two static frames is obvious
- **Generate a little longer than you need.** A 5-second clip trimmed to 4 gives you a cut point. A 4-second clip used whole forces you to cut on its exact first and last frames, which are the weakest
- **Match motion direction across a cut.** Movement left-to-right cutting to right-to-left feels wrong
- **Don't dissolve.** Hard cuts read as intentional; crossfades between generated clips read as a slideshow
- **Vary the shot scale.** Wide, then close, then medium. Three clips at the same scale feel static even with cuts
- **Cover a weak join with b-roll** rather than accepting a visible one

That second point is worth restating: **always generate a second or two of margin.** Trimming is free; regenerating for a cut point is not.

## Text, captions and audio

None of it belongs in the generation.

- **Overlay text in the editor.** Models render text badly, and you'll want to change it per variant anyway
- **Add music and voiceover in the edit.** Don't pass music into `generate_lip_sync` — it interferes with the sync
- **Captions in the middle third** for vertical, not the bottom. See video caption subtitle planning

Generating clean visual beats and adding every text and audio layer afterwards is both cheaper and more flexible.

## Where a single long generation is fine

- **One continuous motion** genuinely designed as one shot — a slow orbit, a long push-in
- **Under about 8 seconds**, where drift hasn't set in
- **Lip sync**, which has to be one continuous clip up to 30s and has no draft path. Longer scripts get segmented instead. See `lip-sync-spokesperson-video`

Everything else assembles better.

## Don't

- **Don't generate a 20-second narrative in one call.**
- **Don't ask for two motions in one beat.**
- **Don't paraphrase the look clause** between beats. Copy it verbatim.
- **Don't generate exactly the duration you need.** Leave trim margin.
- **Don't crossfade.** Hard cuts.
- **Don't render text or music into the clips.**
- **Don't cut between two static frames.** Cut on motion.
- **Don't enhance every beat.** Only the ones you're keeping.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`character-consistency-video`, `draft-then-enhance-workflow`, `text-to-video-prompting`, `async-video-job-orchestration`, `video-hook-first-3-seconds`
