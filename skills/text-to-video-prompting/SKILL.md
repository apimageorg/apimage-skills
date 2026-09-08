---
name: text-to-video-prompting
description: Write text-to-video prompts that produce the motion, camera and subject behaviour you asked for. Use whenever the user is generating video from a text prompt with APImage, gets drifting or morphing or static output, or asks how to prompt for camera moves and motion.
---

# Text-to-Video Prompting

Image prompting describes a frame. Video prompting describes a frame **plus what changes over time**, and the second part is where almost all failures live.

A prompt that reads like an image prompt gets you either a near-static clip or motion the model invented.

## The four things a video prompt must state

Most prompts state one or two and let the model guess the rest. Guessed motion is the drifting, morphing output people complain about.

| Element | State it as | Example |
|---|---|---|
| **Subject** | What is in frame, concretely | "A ceramic mug, steam rising" |
| **Subject motion** | What the subject does over the clip | "steam thickens, then drifts left" |
| **Camera** | The move, or explicitly none | "camera locked, no movement" |
| **Look** | Lighting, lens, grade, mood | "soft morning window light, shallow depth of field" |

Naming the camera is the highest-leverage addition. Without it the model picks a move, and the move it picks is usually a slow drift that makes the clip feel unintentional.

## Camera vocabulary that works

```
camera locked, no movement          — the most useful and least used
slow push in / slow pull out        — dolly toward or away
pan left / pan right                — rotate horizontally
tilt up / tilt down                 — rotate vertically
orbit around subject                — arc, keeps subject centred
tracking shot, follows subject      — camera moves with the subject
handheld, subtle shake              — organic, UGC-appropriate
crane up / crane down               — vertical travel
static tripod                       — same as locked, phrased differently
```

**"Camera locked, no movement" is the fix for most drifting output.** If you don't want a camera move, say so explicitly — an unstated camera is not a static camera.

## Structure

```
[SUBJECT and composition]. [SUBJECT MOTION over the clip].
[CAMERA]. [LIGHTING and LOOK]. [PACE and mood].
```

**Weak — reads like an image prompt:**
```
A skincare serum bottle on a marble surface, soft lighting, luxurious,
high quality, 4k, beautiful
```

That gets a near-static shot with an invented drift. "Luxurious", "high quality", "beautiful" and "4k" contribute nothing about motion.

**Strong:**
```
A glass serum bottle on white marble, dropper lifted just above it.
A single drop falls and lands, spreading slowly. Camera locked, no
movement. Soft directional window light from the left, shallow depth
of field, warm neutral grade. Calm, unhurried pace.
```

Every clause does work: subject, a specific motion with a beginning and end, camera stated, light stated, pace stated.

## Motion has to be achievable in the duration

A 5-second clip cannot contain a three-beat narrative. Prompts that ask for one produce either a rushed mess or the model quietly picking one beat.

| Duration | What fits |
|---|---|
| 4-5s | **One** motion. A drop falls. A hand enters. A lid opens |
| 6-8s | One motion with a beginning and an end, or two small beats |
| 10-12s | Two clear beats, or one slow continuous move |
| 15s+ | A narrative arc — but usually better as separate clips, cut |

**One motion per clip is the reliable unit.** Assemble beats in the edit rather than asking one generation for three of them. See `multi-scene-video-assembly`.

## The failure modes, and the prompt fixes

| Symptom | Cause | Fix |
|---|---|---|
| Slow unintended drift | Camera unstated | Add "camera locked, no movement" |
| Subject morphs or warps | Too much motion for the duration | One motion, or longer clip |
| Nothing happens | No motion described | State what changes, explicitly |
| Wrong subject entirely | Prompt front-loaded with style words | Lead with subject, put style last |
| Faces distort | Face-heavy composition with fast motion | Wider framing, slower motion, or use a reference image |
| Text in frame is garbled | Models render text badly | Don't put text in the render. Add it in the edit |
| Hands look wrong | Hands are hard | Frame them partially, in motion, or avoid close-ups |

**Text and hands are the two things to design around rather than fight.** Overlay text in editing. Keep hands moving and partially out of frame.

## Use `enhance_prompt`

`enhance_prompt` is **free** and it expands a thin prompt into a detailed specification. There's no reason not to run it on a first draft.

```
enhance_prompt(prompt="serum bottle, drop falling, nice lighting")
→ a fuller prompt with camera, lighting and pacing filled in
```

Then edit its output rather than accepting it — it will sometimes add a camera move you don't want. Treat it as a first draft that surfaces the elements you forgot.

## The call

```
generate_video(
  mode="text-to-video",
  model="flux-3-video-draft",
  prompt="A glass serum bottle on white marble... camera locked...",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

Draft first, lock the seed, change one clause at a time. See `draft-then-enhance-workflow` and `seed-locked-iteration`.

## When image-to-video is the better call

If the subject matters — a specific product, a specific person — text-to-video is the wrong mode. It will invent something adjacent.

Generate or supply a still, then animate it. The subject is then fixed and the prompt only has to describe motion. See `image-to-video-animation`.

## Don't

- **Don't leave the camera unstated.** It's the main cause of drift.
- **Don't write image prompts.** Describe change over time.
- **Don't ask for three beats in five seconds.**
- **Don't put style adjectives first.** Subject first, style last.
- **Don't render text into the video.** Overlay it.
- **Don't use text-to-video for a specific product.** Use image-to-video.
- **Don't skip `enhance_prompt`.** It's free.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-to-video-animation`, `seed-locked-iteration`, `draft-then-enhance-workflow`, `multi-scene-video-assembly`
