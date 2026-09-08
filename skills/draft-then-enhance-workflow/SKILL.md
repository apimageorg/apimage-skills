---
name: draft-then-enhance-workflow
description: Iterate video cheaply on flux-3-video-draft and pay full quality only for the winner via enhance_video_draft. Use whenever the user is generating video with APImage, mentions credit cost or running out of credits, is iterating on a prompt, or is about to render variants at full quality.
---

# Draft, Then Enhance

The most expensive habit in APImage video is iterating at full quality. It's also the default, because `generate_video` with a production model is the obvious first call.

flux 3 video draft plus `enhance_video_draft` exists specifically to break that habit, and using it properly changes the cost of a video project by a large multiple.

## The economics

Getting a video right takes somewhere between five and fifteen generations. Prompt wording, motion, composition, framing, pacing — each needs a look.

```
Full-quality iteration:    12 renders × full cost        = 12 units
Draft then enhance:        12 drafts + 1 enhance         ≈ 2-3 units
```

The exact ratio depends on resolution and duration, but the shape holds: **you are paying for judgement calls, and judgement calls do not need 1080p.**

## The loop

```
1. Draft        flux-3-video-draft, shortest useful duration, hd
2. Judge        motion? composition? framing? pacing?
3. Adjust       one variable, same seed
4. Repeat       until the clip is right
5. Enhance      enhance_video_draft on the winning job
```

Step 3 is where most of the value is. Change one thing per iteration and keep the seed fixed, or you can't attribute the improvement. See `seed-locked-iteration`.

## What drafts are good enough to judge

Drafts are lower fidelity, so be clear about what you can and cannot assess from one.

| Judge from a draft | Wait for the enhance |
|---|---|
| Camera motion and path | Fine texture and material detail |
| Composition and framing | Skin and fabric realism |
| Subject behaviour and timing | Small text legibility |
| Pacing across the clip | Reflection and refraction accuracy |
| Whether the prompt was understood | Fine edge quality |
| Aspect ratio and safe areas | Colour grading nuance |

Everything in the left column is where video generation actually fails. If the camera drifts, the subject morphs or the pacing is dead, a full-quality render of the same prompt has the same problem at higher cost.

**Don't reject a draft for softness.** That's the draft doing its job. Reject it for motion, composition or comprehension.

## The calls

**Draft:**

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=["<still>"],
  prompt="...",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
→ returns job_id
```

**Poll:**

```
get_video_generation(id="<job_id>")
```

**Enhance the winner:**

```
enhance_video_draft(id="<winning draft job_id>")
→ returns a new job_id. Poll that one too
```

`enhance_video_draft` re-renders the *same clip* at full quality. It's not a new generation from the prompt — which is the point. The motion you approved is the motion you get.

## Keep the job IDs

The whole workflow depends on being able to point at the winning draft, which means keeping the IDs.

```
list_video_generations(status="completed", mode="text-to-video")
```

That's paginated history filterable by status and mode. Use it to find a draft from earlier in a session rather than regenerating one you liked and lost.

**Log the ID with the prompt** as you iterate. Six drafts in, "the third one was right" is not actionable unless you wrote down which job that was.

```
draft 1  job_a91f  seed=4271  "slow push in"           → drifts left
draft 2  job_b02c  seed=4271  "camera locked"          → static, dead
draft 3  job_c73e  seed=4271  "locked, subject moves"  → this one ✓
```

## When to skip drafting

Drafting has overhead, and it isn't always worth it.

- **A prompt you've used before at a known seed.** You already know what it does
- **Lip sync.** `generate_lip_sync` requires seedance 2 0 and there's no draft path. Script tightly instead, because it bills at 3 credits/second
- **A one-off where you'll accept the first output.** Rare, but it happens
- **Very short clips at low resolution**, where the draft and the final cost nearly the same

Everything else — especially anything going into paid media, or any batch of variants — should draft first.

## Combine with variant testing

Drafting makes variant testing affordable, which is the second-order benefit and the bigger one.

```
6 hook drafts  →  judge  →  enhance the 2 best  →  test those 2 in market
```

Six drafts plus two enhances costs meaningfully less than six full renders, and it produces two tested creatives rather than six untested ones. See `video-ab-testing-variants`.

## Cost checks

- `check_credits` before a batch. It reports balance, monthly usage against quota and tier
- **Failed renders are already refunded.** Don't chase them or re-run defensively
- Video rate limit is 30 requests/minute with a concurrent in-flight cap — a large draft batch should be queued, not fired at once
- Polling with `get_video_generation` and `list_video_generations` is **free**. Poll freely; never regenerate to check

## Don't

- **Don't iterate at full quality.** The whole point of this skill.
- **Don't reject a draft for being soft.** Judge motion and composition.
- **Don't lose the job IDs.** Log them with the prompt.
- **Don't re-render from the prompt** when `enhance_video_draft` will upgrade the clip you already approved.
- **Don't fire fifteen drafts simultaneously.** There's a concurrency cap.
- **Don't poll by regenerating.** Polling is free; generation is not.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`video-model-selection`, `async-video-job-orchestration`, `seed-locked-iteration`, `video-credit-cost-management`, `video-ab-testing-variants`, `ugc-batch-testing`
