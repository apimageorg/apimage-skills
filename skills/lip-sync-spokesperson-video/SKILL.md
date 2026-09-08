---
name: lip-sync-spokesperson-video
description: Generate talking-avatar and spokesperson video with generate_lip_sync, including the 30-second cap, the 3-credits-per-second cost and the face-image requirements. Use whenever the user wants a talking head, AI spokesperson, avatar presenter, voiceover-to-face video, or mentions lip sync.
---

# Lip-Sync Spokesperson Video

`generate_lip_sync` turns a face image plus audio into a talking avatar. It's the most constrained tool in APImage and the most expensive per second, so the craft is mostly in what you decide *before* generating.

## The constraints, up front

| Constraint | Value | Consequence |
|---|---|---|
| Model | seedance 2 0 only | No draft path. No cheap iteration |
| Cost | **3 credits per second** | 30s = 90 credits |
| Max duration | **30 seconds** | Longer scripts must be split |
| Input | Face image + audio | Both have to be right first |

**90 credits for a 30-second clip** is a third of a Starter plan's monthly allowance. That reframes the work: you're not iterating your way to a good clip, you're getting the inputs right and generating once.

## Script to the second, first

At 3 credits/second, every unnecessary word is money. Write the script, read it aloud with a timer, and cut.

| Duration | Realistic word count | Use for |
|---|---|---|
| 8s | ~20 words | A single claim or hook |
| 12s | ~30 words | Hook plus one supporting point |
| 15s | ~38 words | Problem, then solution |
| 20s | ~50 words | Three beats, tightly |
| 30s | ~75 words | A full short ad. The cap |

Cut before generating: greetings, "in this video", self-introduction, brand name repetition, and any sentence that doesn't advance the point. A 30-second script that says what a 15-second one could costs double for nothing.

See talking head scripting for structure.

## The face image

The output quality is largely decided here, and a bad source image cannot be fixed by regenerating.

**What works:**
- Front-facing or near-front, head and shoulders framing
- Both eyes visible, neutral or slightly-open mouth
- Even, soft lighting on the face — no hard shadow across the mouth
- Sharp focus on the face specifically
- Uncluttered background
- Natural, relaxed expression

**What breaks it:**
- Strong profile or three-quarter angle
- Mouth wide open, or teeth fully exposed
- Hard shadow across the lower face
- Glasses with heavy glare
- Hair or a hand across the mouth
- Motion blur
- Low resolution or a heavily compressed source

You can *generate* the face image first with `generate_image` and iterate cheaply there — which is the right order, because image iterations cost 1-9 credits and lip-sync iterations cost 90.

```
1. generate_image     → iterate to a good presenter portrait (cheap)
2. create_brand_asset → save it as a reusable character
3. generate_lip_sync  → once, with the approved face
```

Saving the approved face as a brand asset means every future clip uses the same presenter. 

## The audio

- **Clean audio only.** Background noise, room reverb and clipping all degrade the sync
- **Consistent pace.** Very fast delivery syncs worse than measured delivery
- **Clear consonants.** Mumbled plosives produce mushy mouth shapes
- **No music in the track you pass in.** Add music in the edit, afterwards, or it interferes with the sync
- **Match the language to the face** if the audience will notice

Generate or record the audio, listen to it end to end, and only then spend the 3-credits-per-second.

## The call

```
generate_lip_sync(
  face_image="<url or asset id of the approved portrait>",
  audio="<clean audio, under 30s>"
)
→ returns job_id. Poll with get_video_generation
```

Video is always async. Poll — never re-call because nothing came back. See `async-video-job-orchestration`.

## Splitting longer scripts

For anything over 30 seconds, generate segments and assemble.

```
Segment 1  0-14s   hook + problem       → 42 credits
Segment 2  14-28s  solution + proof     → 42 credits
Segment 3  28-38s  offer + CTA          → 30 credits
```

Two things make the segments cut together cleanly:

- **Same face image and same framing** across all segments
- **Segment on natural pauses** in the script, not mid-sentence

Then cut on the pauses, or cover the joins with b-roll. Covering a join with a product shot is often better than the join being visible at all. See `multi-scene-video-assembly`.

## When a talking head is the wrong choice

Worth asking, because it's the most expensive format on the platform.

- **Product demonstration** — show the product. product demo video is cheaper and more persuasive
- **Transformation or result** — show the before and after. before after transformation video
- **Anything the visual can carry alone** — captions over b-roll costs a fraction
- **Faceless content strategy** — see faceless video automation

A talking head earns its cost when the *message needs a person*: trust, testimony, explanation, personality. It's waste when it's a person narrating something the camera could just show.

## Rights and disclosure

Two things that matter and are easy to get wrong.

**Likeness.** Do not use a real person's face without permission. That includes public figures, stock portraits whose licence excludes AI or synthetic use, and anyone whose photo you happen to have. Generate a synthetic presenter instead — it's cheap, it's consistent, and it's yours.

**Disclosure.** An AI-generated spokesperson presented as a real customer testimonial is a false endorsement, and in several jurisdictions that's an advertising violation rather than a stylistic choice. Platforms also increasingly require synthetic-media labelling on ads. Label it. 

## Don't

- **Don't iterate on lip sync.** 3 credits/second. Iterate on the face image instead.
- **Don't use a profile or hard-shadowed face image.**
- **Don't pass audio with music in it.**
- **Don't script past what the duration allows.** Read it with a timer.
- **Don't use a real person's face without permission.**
- **Don't present a synthetic presenter as a real customer.**
- **Don't use a talking head for something the visual could show.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`character-consistency-video`, `multi-scene-video-assembly`, `video-credit-cost-management`
