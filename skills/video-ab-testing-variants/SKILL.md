---
name: video-ab-testing-variants
description: Generate and test creative variants systematically so you learn what works instead of guessing, isolating one variable per variant. Use whenever the user is testing ad creative, generating multiple versions, mentions A/B testing or variants or creative testing, or has one video and wants to know if it's the best one.
---

# Creative Variant Testing

Generative video makes variants nearly free compared to filming them, which changes the economics of creative testing completely. Most people don't exploit that — they generate one clip, judge it by opinion, and run it.

The reason to test isn't rigour for its own sake. It's that **performance variance between creatives is enormous and almost impossible to predict**, including by people who are good at this.

## Isolate one variable

The rule that makes testing produce learning rather than noise.

```
Wrong:
  Variant A: hook 1, presenter A, kitchen, upbeat pace
  Variant B: hook 2, presenter B, outdoors, calm pace
  → B wins. You have learned nothing you can reuse.

Right:
  Variant A: hook 1, presenter A, kitchen, upbeat
  Variant B: hook 2, presenter A, kitchen, upbeat
  Variant C: hook 3, presenter A, kitchen, upbeat
  → C wins. Hook 3 is now a reusable asset.
```

Change one thing. The seed and every other parameter stay fixed. See `seed-locked-iteration`.

## What to test, in order of impact

Test in this order, because the effect sizes differ by an order of magnitude.

| Variable | Impact | Notes |
|---|---|---|
| **Hook (first 3s)** | Largest, by far | Where nearly all variance lives |
| **Offer / claim** | Large | What you're actually saying |
| **Format** | Large | Demo vs testimonial vs transformation |
| **Presenter or subject** | Medium | Who is on screen |
| **Pace and duration** | Medium | 6s vs 12s changes completion rate |
| **Setting** | Small-medium | Kitchen vs outdoors |
| **Grade and look** | Small | Warm vs cool |
| Resolution | Negligible | Stop testing this |

**Test hooks first and test them most.** Six hooks against one body teaches you more than six complete concepts, and costs less.

## The efficient structure

Because a hook is a separate 4-second clip, you can test hooks without re-rendering bodies.

```
1 body clip        (enhanced, full quality)   → the constant
6 hook drafts      (4s each, draft model)     → the variable
= 6 testable creatives for roughly the cost of two full renders
```

```
generate_video(mode="image-to-video", model="flux-3-video-draft",
               reference_images=[still], prompt=HOOKS[i],
               aspect_ratio="9:16", duration=4, seed=4271)
```

Same seed, same reference, same duration, same everything except the hook prompt. Then enhance the two best and cut each onto the same body. See `draft-then-enhance-workflow` and `multi-scene-video-assembly`.

## How many variants

| Situation | Variants |
|---|---|
| New product, no prior data | 6-10 hooks, 2-3 formats |
| Known winner, incremental | 3-4, one variable |
| Scaling a proven creative | 4-6 fresh hooks on the same body |
| Creative fatigue on a running ad | 4+, genuinely different formats |

**Fresh hooks on a proven body is the cheapest way to extend a creative's life.** Ad fatigue usually shows up as hook fatigue first — the body still works, the opening has been seen.

## Judge before you spend media budget

Two stages of judgement, and the first is free.

**Stage 1, internal.** Watch all variants **muted, on a phone, in sequence**. Discard anything that fails the three-second test. This removes the obviously weak ones before they cost media spend.

**Stage 2, in market.** The only judgement that counts. Internal preference correlates poorly with performance — including experienced internal preference. Discard the bottom half internally, then let the market rank the rest.

Don't over-trust stage 1. Its job is removing the broken, not picking the winner.

## Sample size, honestly

The uncomfortable part. Distinguishing a 2% from a 2.4% click-through rate needs far more impressions than most budgets allow, and calling a winner early on small numbers is the most common mistake in creative testing.

Practical guidance:

- **Test for a big difference, not a small one.** Genuinely different creative concepts produce differences you can see. Colour-grade variants don't
- **Look at early-funnel metrics first.** Three-second retention and hook rate move faster and need less volume than conversion
- **Don't declare a winner on a day of data**
- **Accept ties.** If two creatives perform the same, run both — variety delays fatigue

If the budget can only detect large differences, only test large differences. Testing subtle variants on small spend produces confident conclusions from noise.

## Log the variants

A test you can't reconstruct teaches nothing. Log the job ID, the variable and the result.

```
job_a91f  hook="mid-action pour"        3s-ret 61%   ✓ winner
job_b02c  hook="result first"           3s-ret 54%
job_c73e  hook="problem in frame"       3s-ret 58%
job_d14a  hook="direct address"         3s-ret 49%
job_e55b  hook="slow push in"           3s-ret 31%   ← the default. Worst
```

`list_video_generations(status="completed")` is free and paginated — use it to reconstruct a batch rather than your memory.

Over a few rounds this becomes a hook library: patterns that work for your product, evidenced. That library is worth more than any individual winning creative. 

## Don't

- **Don't change two variables at once.**
- **Don't test at full quality.** Draft the variants, enhance the winners.
- **Don't test resolution.**
- **Don't trust internal preference** as a predictor.
- **Don't call a winner on a day of data.**
- **Don't test subtle variants on small budget.** You'll be reading noise.
- **Don't discard the log.** The pattern library is the compounding asset.
- **Don't re-render bodies** to test hooks.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`video-hook-first-3-seconds`, `draft-then-enhance-workflow`, `seed-locked-iteration`, `video-credit-cost-management`, `ugc-batch-testing`
