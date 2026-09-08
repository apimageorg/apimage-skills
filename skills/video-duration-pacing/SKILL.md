---
name: video-duration-pacing
description: Choose clip duration and cut rhythm deliberately, since duration drives both cost and completion rate. Use whenever the user is deciding how long a video should be, mentions pacing or retention or completion rate, or has video that drags or feels rushed.
---

# Duration and Pacing

Duration is the parameter people set by feel and then pay for twice — once in credits, once in completion rate. APImage supports 4 to 30 seconds per generation, model-dependent, and cost scales with it directly.

The useful frame: **duration is a budget for beats.** Decide how many beats the story needs, then pick the duration that fits them.

## What fits in what

| Duration | Beats | Use for |
|---|---|---|
| **4-5s** | One | A hook. A single motion. A product reveal. The cheapest useful unit |
| **6-8s** | One with a beginning and end, or two small | A standalone social clip |
| **10-12s** | Two clear beats | Hook plus payoff |
| **15-20s** | Three | Needs a real arc or it drags |
| **20-30s** | Four+ | Only with a script that earns it. Usually better assembled |

**One motion per generation is the reliable unit.** Asking a 5-second clip for a three-beat narrative produces either a rushed mess or the model quietly picking one beat and ignoring the rest.

Above about 10 seconds, generate beats separately and cut them. It's cheaper, it drifts less, and the pacing becomes yours rather than the model's. See `multi-scene-video-assembly`.

## Total length by destination

Separate decision from clip length, and it's about completion rate.

| Destination | Total | Why |
|---|---|---|
| TikTok / Reels / Shorts | **6-15s** | Completion rate is a ranking input. Shorter completes |
| Paid social | **6-12s** | Even shorter. You're buying attention |
| Story | 5-8s per card | Card-based |
| YouTube standard | 30s-2min+ | Different viewing contract |
| Website hero loop | 4-8s, seamless | Loops, so short is fine |
| Email | 5-8s | Often auto-playing, muted |

**A 30-second organic short-form clip is usually a 12-second clip with 18 seconds of drag.** Completion matters more than content volume on these platforms — a fully-watched 8-second clip outperforms a half-watched 20-second one, both algorithmically and in practice.

The instinct to include more because you generated it is the enemy here. Cut it.

## Pacing inside the clip

Cut rhythm carries more of the feel than any individual shot.

| Rhythm | Cut every | Reads as |
|---|---|---|
| Frantic | 0.5-1s | Energetic, trend-native. Exhausting past 10s |
| Fast | 1-2s | Standard short-form. Safe default |
| Medium | 2-4s | Considered, explanatory |
| Slow | 4s+ | Premium, calm. Risky on short-form |

**Fast — roughly 1.5 to 2 seconds per shot — is the default that works** for most short-form. That means a 10-second clip is five or six shots, which means five or six generations of 3-4 seconds each with trim margin.

Vary it rather than holding one rhythm: fast through the hook, slower on the payoff. A constant cut rate feels mechanical even when the individual shots are good.

## Always generate trim margin

The single most useful production habit in this skill.

```
Need 4s in the edit  →  generate 5-6s
Need 3s hook         →  generate 4-5s
```

Reasons:

- The first and last half-second of a generation are the weakest — motion is often settling or drifting
- A cut needs a frame to land on, and you want a choice of frames
- Trimming is free; regenerating for a cut point is not

Generating exactly the length you need forces you to use the weakest frames as your cut points.

## Where the duration budget goes

Given a fixed credit budget, duration competes with variant count — and variants win.

```
Same budget:
  Option A:  2 × 15s clips
  Option B:  6 × 5s clips
```

Option B gives you a hook library, testable variants, and material to assemble. Option A gives you two guesses. See `video-ab-testing-variants` and `video-credit-cost-management`.

## Retention shape

Short-form retention curves have a predictable shape, and pacing decisions should respond to it.

```
0-1s     Steepest drop. The hook decides everything
1-3s     Still falling fast
3-7s     Flattens if the hook landed
7-12s    Gradual decline
12s+     Falls off unless there's a genuine reason to stay
```

Two implications:

**Front-load everything that matters.** The payoff at second 18 is seen by a fraction of the people who saw second 2. If the product only appears at the end, most viewers never see it.

**Earn every second past 12.** A specific unresolved question, a build, a genuine reveal. "More information" is not a reason to stay.

## Loops

For a website hero or a background element, a seamless loop makes a 5-second clip feel unlimited.

```
prompt="...motion that returns to its starting position by the end
        of the clip. First and last frames match."
```

It rarely comes out perfectly seamless. Fix it in the edit with a short crossfade at the loop point, or pick a motion that's naturally cyclical — steam, water, fabric, particles.

## The generation call

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[still],
  prompt="One motion: hands lift the product and hold it steady. "
         "Camera locked. Everything else still.",
  aspect_ratio="9:16",
  duration=5,          # need 4 in the edit, generating 5
  seed=4271
)
```

Draft at the target duration — pacing is exactly the kind of thing a draft is good enough to judge. See `draft-then-enhance-workflow`.

## Don't

- **Don't ask one generation for three beats.**
- **Don't generate exactly the duration you need.** Leave trim margin.
- **Don't default to 15 or 30 seconds** because it's available.
- **Don't put the payoff at second 18.**
- **Don't hold one cut rhythm** through a whole clip.
- **Don't include content because you generated it.** Cut it.
- **Don't spend the budget on duration** when variants move performance more.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`multi-scene-video-assembly`, `video-hook-first-3-seconds`, `video-model-selection`, `video-credit-cost-management`, `video-ab-testing-variants`
