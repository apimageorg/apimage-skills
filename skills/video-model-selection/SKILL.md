---
name: video-model-selection
description: Choose the right APImage video model for a job — seedance-2-0, seedance-2-0-fast, seedance-2-5, flux-3-video or flux-3-video-draft — and the resolution and duration that go with it. Use whenever the user is generating video with APImage, asks which model to use, mentions credit cost or render time, or is iterating on a clip that isn't working.
---

# Choosing a Video Model

Five models, and picking wrong costs either credits or quality. The decision is almost always about **where you are in the iteration loop**, not about which model is "best".

## The models

| Model | Use it for | Notes |
|---|---|---|
| flux 3 video draft | Iterating on prompt, motion and composition | Cheapest. Then `enhance_video_draft` the winner |
| flux 3 video | Final renders where you want FLUX motion | `hd` (720p) or `fhd` (1080p) |
| seedance 2 0 | Default. Broad capability, **and the only lip-sync model** | 480p / 720p / 1080p |
| seedance 2 0 fast | Volume batches, variant testing | Faster, lower cost, slightly less fidelity |
| seedance 2 5 | Highest Seedance fidelity | Use when the clip is going to spend money on ads |

## The workflow that saves the most credits

Iterating at full quality is the single most expensive mistake in APImage video, and it's the default behaviour.

```
1. Draft      flux-3-video-draft, 4-5s, low res     → cheap iterations
2. Judge      Is the motion right? The composition?  → discard most
3. Enhance    enhance_video_draft on the winner      → full quality
```

`enhance_video_draft` re-renders a flux 3 video draft clip at full quality. That means you can burn ten cheap drafts finding the prompt that works and pay full price once — rather than paying full price eleven times.

**Iterate on drafts. Enhance the winner.** If you take one thing from this skill, take that.

## The lip-sync constraint

`generate_lip_sync` requires seedance 2 0. If the video needs a talking presenter, that's the model, and the decision is made for you.

Lip sync bills at **3 credits per second** and caps at **30 seconds**. A 30-second talking clip is 90 credits, which on a Starter plan is close to a third of the monthly allowance. Script tightly. See `lip-sync-spokesperson-video`.

## Resolution: match the destination

| Destination | Resolution | Why |
|---|---|---|
| Draft iteration | 480p (Seedance) or `hd` (FLUX) | You're judging motion, not pixels |
| Organic social | 720p / `hd` | Platforms recompress hard. 1080p is mostly wasted |
| Paid social | 1080p / `fhd` | Ad platforms are less forgiving, and CPMs justify it |
| Anything reused in a longer edit | 1080p / `fhd` | You can downscale later, never up |

**Organic social at 1080p is usually wasted credits.** TikTok, Reels and Shorts all recompress aggressively, and the difference between a 720p and a 1080p source survives that badly. Spend the difference on more variants instead. See `video-ab-testing-variants`.

## Duration

4 to 30 seconds, model-dependent. Cost scales with it, so duration is a budget decision as much as a creative one.

- **4-6s** — a single beat. Hook, product reveal, one transformation. The most credit-efficient unit
- **8-12s** — hook plus payoff. The sweet spot for a standalone social clip
- **15-20s** — needs a real narrative arc or it drags
- **20-30s** — only with a script that earns it. Usually better as two clips assembled

Generating one 24-second clip when you needed three 8-second beats is both more expensive and less editable. See `multi-scene-video-assembly`.

## The call

```
generate_video(
  mode="text-to-video",              # or image-to-video, video-to-video
  model="flux-3-video-draft",        # draft first
  prompt="...",
  aspect_ratio="9:16",
  resolution="hd",
  duration=6,
  seed=1234,                          # lock it once the motion is right
  reference_images=[...],             # 9-30 depending on model
  webhook_url="https://..."          # optional, avoids polling
)
```

**Video always runs asynchronously.** The call returns a job ID immediately. Poll with `get_video_generation` — do not call `generate_video` again because nothing came back, or you'll pay twice for the same clip. See `async-video-job-orchestration`.

## Seeds

Once the motion and composition are right, **lock the seed** and change one thing at a time. Without a fixed seed every regeneration is a new roll and you can't tell whether your prompt edit helped or the dice did.

```
seed=1234, prompt="...slow push in..."      → good motion, wrong lighting
seed=1234, prompt="...slow push in, golden hour..."   → same motion, new lighting
```

See `seed-locked-iteration`.

## Cost discipline

- Check `check_credits` before a batch. Video is the most expensive thing on the platform
- Draft, then enhance. Never iterate at full quality
- Prefer seedance 2 0 fast for variant volume
- Shorter clips, assembled, beat one long clip
- **Failed renders are already refunded** — you don't need to chase them
- Rate limit on video is 30 requests/minute, with a concurrent in-flight cap. Queue accordingly

## Don't

- **Don't iterate at full quality.** Draft then enhance.
- **Don't re-call `generate_video` while a job is pending.** Poll instead. You'll be billed twice.
- **Don't render organic social at 1080p** by reflex.
- **Don't use a model other than seedance 2 0 for lip sync.** It won't work.
- **Don't leave the seed unset while iterating.** You can't attribute the improvement.
- **Don't generate one long clip** when the edit wants three short ones.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

Codex, via `mcp-remote`:

```toml
[mcp_servers.apimage]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.apimage.org/mcp", "--header", "Authorization: Bearer sk_your_api_key"]
```

Keys come from the [APImage dashboard](https://apimage.org).

## Related skills

`draft-then-enhance-workflow`, `async-video-job-orchestration`, `seed-locked-iteration`, `video-credit-cost-management`, `aspect-ratio-strategy`
