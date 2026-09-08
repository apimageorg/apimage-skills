---
name: ugc-performance-iteration
description: Diagnose which part of a UGC creative is failing from the metric that dropped, and fix that part rather than remaking the video. Use whenever UGC creative underperforms, an ad fatigues, results plateau, or the user is about to rebuild a creative from scratch.
---

# Iterating on Performance

When a UGC ad underperforms, the reflex is to make a new one. That's usually the most expensive available response and the least informative. Short-form has a small number of distinct failure points, each with a distinct metric signature, and each fixable independently.

**Read the funnel, find the beat that's failing, regenerate that beat.**

## The diagnostic table

| Symptom | Failing beat | Fix |
|---|---|---|
| Impressions fine, 3s retention low | **The hook** | New hooks on the same body |
| 3s fine, drops hard at 5-8s | **The second beat** | The hook over-promised, or the middle is slow |
| Watched through, no clicks | **The CTA, or no reason to act** | Clearer action, or the offer |
| Clicks but no conversion | Not the creative | Landing page, price, or targeting |
| Good at launch, decaying | **Fatigue** | Fresh hooks, then a fresh face |
| Never worked at all | **The concept or the audience** | Different format, or different targeting |
| Works on one platform only | Register mismatch | Re-cut per platform |
| High spend, no data | Budget spread too thin | Fewer cells |

**The most common misdiagnosis is treating a hook problem as a concept problem.** Low three-second retention with healthy impressions means the opening failed. The rest of the video was never seen, so remaking it changes nothing.

## Fix the beat, not the video

The economics that make this worth doing.

```
Rebuild the creative       full render + enhance + lip sync
Regenerate the hook only   one 4s draft, cut onto the proven body
```

Same references, same seed, same look clause, same body. Only the failing beat is regenerated. That is roughly an order of magnitude cheaper and it isolates the variable, so you learn something.

```python
# the body is already enhanced and proven. Don't touch it.
new_hook = generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=CREATOR + [PRODUCT, BG],
  prompt=f"{NEW_HOOK_ACTION} {LOOK} Everything else stays still.",
  aspect_ratio="9:16", resolution="hd", duration=4, seed=4271)
```

**Copy the look clause verbatim** so the new beat cuts against the old body seamlessly. Paraphrasing it produces a visible seam. See `ugc-character-consistency` and `ugc-hook-library`.

## Fatigue has an order

Creative decay isn't uniform. It happens in a sequence, and each stage has a cheaper fix than the next.

```
1. Hook fatigue      first, and fastest        → fresh hooks
2. Face fatigue      next                      → a different creator
3. Format fatigue    later                     → a different format
4. Concept fatigue   last                      → a new concept
```

Work down the list in order. Most teams jump to step four, which is the most expensive and often unnecessary — the concept was fine and the audience had simply seen that opening thirty times.

**Keep two unused personas and a stocked hook library in reserve** precisely so steps one and two are same-day operations. See `ugc-multi-creator-variants`.

## What the metrics actually tell you

- **Hook rate and 3-second retention** move fastest and need the least volume. Read these first, and they're enough to kill a variant
- **Watch-through** diagnoses the middle
- **Click-through** diagnoses the CTA and the offer
- **Conversion rate** is mostly not about the creative. If clicks are healthy and conversion isn't, look at the page and the price
- **Frequency** is the fatigue signal. Rising frequency with falling retention is textbook

**Judge muted, on a phone, at feed size** before you look at any number. Half of underperformance is visible in ten seconds of watching it the way the audience does.

## The pre-spend checks

Cheaper than diagnosing after the fact.

```
[ ] Watched muted on a phone at feed size
[ ] Product identifiable at thumbnail size
[ ] Captions clear of the bottom fifth and the right edge
[ ] Hands checked at full size, high-motion frames
[ ] Label undistorted through the motion
[ ] Face consistent across cut beats
[ ] Something happens in the first frame. No slow push-in
[ ] Disclosure on screen in the first three seconds
```

The slow push-in deserves its own line: it's what the model produces when unprompted, and in logged tests it loses to a deliberate opening by a wide margin. If your hook is a slow push-in, you have a hook problem regardless of what the metrics say yet. See `video-hook-first-3-seconds`.

## Keep the record

Iteration without a log is guessing with extra steps.

```
Round  Change            3s-ret   CVR    Verdict
1      original           38%     1.1%   baseline
2      mid-sentence hook  61%     2.0%   hook was the problem
3      creator C          64%     2.4%   face mattered too
4      shorter middle     63%     2.6%   pacing helped conversion
5      new CTA            63%     2.9%   keep
```

Two things it gives you: you stop re-testing things that already failed, and you accumulate product-specific patterns that transfer to the next campaign. That transfer is the actual compounding asset — individual creatives fatigue, patterns don't.

`list_video_generations(status="completed")` is free and paginated. Use it to reconstruct which render was which. See `ugc-batch-testing`.

## When the creative isn't the problem

Worth saying plainly, because a lot of creative iteration is spent on non-creative problems.

- **Clicks healthy, conversion poor** → landing page, price, stock, checkout
- **Nothing works across every variant** → audience or offer, not creative
- **Works organically, fails paid** → targeting, or the disclosure changed the read
- **Sudden collapse overnight** → check for a policy action or a review flag before rebuilding
- **Good CVR, bad ROAS** → economics, not creative

**Check for an account or policy action before diagnosing a creative.** A restricted ad account looks exactly like creative failure in the numbers, and no amount of regeneration fixes it. See `video-brand-safety-moderation`.

## Don't

- **Don't rebuild the video** when one beat is failing.
- **Don't treat low 3s retention as a concept problem.**
- **Don't jump to a new concept** before fresh hooks and a fresh face.
- **Don't paraphrase the look clause** when regenerating a beat.
- **Don't iterate creative** on a conversion problem.
- **Don't call a result in a day.**
- **Don't skip the muted phone check.**
- **Don't diagnose before checking** for a policy action.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-hook-library`, `ugc-batch-testing`, `video-ab-testing-variants`, `ugc-multi-creator-variants`, `video-hook-first-3-seconds`, `video-credit-cost-management`
