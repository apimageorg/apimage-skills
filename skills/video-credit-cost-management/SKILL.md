---
name: video-credit-cost-management
description: Control APImage credit spend — what costs what, where the waste is, and how to plan a batch against a monthly quota. Use whenever the user mentions credits, cost, running out of credits, pricing or budget, or is planning a batch of generations.
---

# Credit and Cost Management

Video is by far the most expensive thing on APImage, and the default working style — iterating at full quality — burns a monthly allowance in an afternoon.

Almost all waste comes from four habits, and all four are fixable.

## What costs what

| Operation | Cost |
|---|---|
`enhance_prompt` | **free** |
`get_image_generation`, `list_image_generations` | **free** |
`get_video_generation`, `list_video_generations` | **free** |
`list_brand_assets`, `get_brand_asset`, `create_brand_asset`, `update_brand_asset`, `delete_brand_asset` | **free** |
`check_credits` | **free** |
`analyze_image` | 1 credit |
`remove_background` | 2 credits |
`replace_background` | 3 credits |
`generate_image` | 1-9 credits, model and resolution dependent |
`edit_image` | generation credits |
`generate_brand_asset` | generation credits |
`generate_lip_sync` | **3 credits per second** |
`generate_video` | metered, by model, resolution and duration |
`enhance_video_draft` | metered |

Two things to internalise from that table:

**Everything diagnostic is free.** Polling, history, prompt expansion, asset management, credit checks. There is no cost reason to avoid any of them, and every reason to use them instead of regenerating.

**Lip sync is priced per second.** 30 seconds is 90 credits — a third of a Starter month's 300. Script length is a budget decision.

## Plan against the quota

| Plan | Monthly credits | Roughly |
|---|---|---|
| Starter | €19 / 300 | ~2 videos |
| Plus | €47 / 1,200 | ~11 videos |
| Ultra | €99 / 3,000-9,000 | ~28+ videos |

Check before a batch, not after:

```
check_credits()
→ balance, monthly usage against quota, subscription tier
```

Then budget the batch. If you have 400 credits and want six finished clips, you cannot afford six full-quality renders plus iterations. You can afford twenty drafts and six enhances.

## The four wastes

**1. Iterating at full quality.** The big one. Ten full renders to find the right prompt, when ten drafts plus one enhance gets you there for a fraction.

```
Full-quality iteration:  12 × full cost
Draft then enhance:      12 drafts + 1 enhance
```

See `draft-then-enhance-workflow`.

**2. Regenerating instead of polling.** Video is always async. `generate_video` returns a job ID, not a video. Calling it again because nothing came back bills you twice for the same clip. Polling is free.

See `async-video-job-orchestration`.

**3. Rendering longer and higher than the destination needs.** A 20-second 1080p clip for an organic TikTok that will be recompressed and watched for 4 seconds. Duration and resolution both scale cost directly.

**4. Iterating video when the problem is the still.** If the subject is wrong, fix it in `generate_image` at 1-9 credits rather than in `generate_video` at many times that. Image iteration is the cheap half of the job.

## The efficient production pattern

```
1. enhance_prompt          free      → fill in what the prompt is missing
2. generate_image          1-9       → iterate the still cheaply
3. edit_image              gen       → relight, adjust, upscale as needed
4. create_brand_asset      free      → save the approved subject
5. generate_video (draft)  low       → iterate motion cheaply, locked seed
6. enhance_video_draft     metered   → full quality, once
```

Everything expensive happens once, at the end, on something already approved.

## Failed renders are refunded

Stated plainly in the platform docs: **failed renders are already refunded.** That has two practical consequences.

- **Don't reconcile credits** after a failure. It's handled
- **Don't defensively re-run** a job that failed. Read the error, fix the input, run once

Failures come back as MCP tool errors carrying the upstream HTTP status, so they're diagnosable. A 400 or 422 is deterministic — retrying unchanged produces the same failure. See `async-video-job-orchestration`.

## Rate limits shape a batch

| Endpoint | Per minute |
|---|---|
| `/generations`, `/brand-assets` | 120 |
| `/ai-image-generate` | 60 |
| Video, background tools | 30 |
| Concurrent in-flight video jobs | Capped |

The concurrency cap means a batch has to be queued rather than fired. Rejected requests aren't billed, but they do waste your time. Submit in waves of about three and raise it only if nothing is being rejected. 

## Where to spend, when you have to choose

Given a fixed budget, the priority order that produces the best outcome:

1. **More variants over higher resolution.** Six 720p creatives beat two 1080p ones, because performance variance between creatives dwarfs the quality difference
2. **Hook iterations over body iterations.** Almost all short-form performance variance is in the first three seconds. See `video-hook-first-3-seconds`
3. **Image quality over video length.** A great still animated for 5 seconds beats a mediocre still animated for 15
4. **Full quality on winners only.** Everything else stays a draft

That first line is the one worth arguing with a stakeholder about. Spending the budget on fidelity rather than on variants optimises the thing that matters least.

## Don't

- **Don't iterate at full quality.**
- **Don't regenerate to check status.** Polling is free.
- **Don't render longer or higher than the destination needs.**
- **Don't fix a subject problem in video.** Fix it in the image.
- **Don't reconcile credits for failed renders.** Already refunded.
- **Don't retry deterministic errors** unchanged.
- **Don't fire a whole batch at once.** There's a concurrency cap.
- **Don't spend the budget on resolution** when variants are what move performance.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`draft-then-enhance-workflow`, `async-video-job-orchestration`, `video-model-selection`, `video-ab-testing-variants`, `ugc-batch-testing`
