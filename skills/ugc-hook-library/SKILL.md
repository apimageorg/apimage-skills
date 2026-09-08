---
name: ugc-hook-library
description: Build a tested, reusable library of UGC hooks — the first three seconds that carry nearly all short-form performance variance. Use whenever the user needs hooks, openings for UGC ads, is testing creative, or has ads that get impressions but no watch time.
---

# Building a Hook Library

Almost all performance variance in short-form lives in the first three seconds. That makes hooks the highest-leverage thing to generate, to test, and — critically — to **keep a record of.**

A tested hook library is a compounding asset. Individual winning creatives fatigue; a library of patterns that work for your product doesn't.

## Generate hooks as standalone 4-second clips

The structural insight that makes hook testing affordable.

```
1 body clip        enhanced, full quality   → the constant
8 hook drafts      4s each, draft model     → the variable
= 8 testable creatives for a fraction of eight full renders
```

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[CREATOR_ASSET, PRODUCT_ASSET],
  prompt=f"{HOOKS[i]} {LOOK} Handheld, slight shake, ordinary "
         f"kitchen behind, natural window light. Not studio.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=4,
  seed=4271
)
```

Same seed, same references, same duration, same look clause. **Only the hook action and line change.** Then enhance the two best and cut each onto the same body. See `video-ab-testing-variants` and `draft-then-enhance-workflow`.

## The hook patterns

Twelve structures that work across products. These are formats, not scripts — fill them with your specifics.

| Pattern | Opening | Why it works |
|---|---|---|
| **Mid-sentence** | Already talking, no greeting | No dead frames. Feels overheard |
| **Problem stated** | "My [X] was doing [specific thing]" | Recognition |
| **Result first** | Shows the outcome, then "here's how" | Payoff promised immediately |
| **Contrarian** | "Everyone says [X]. That's wrong" | Curiosity, mild conflict |
| **Mistake** | "I was doing this completely wrong" | Self-deprecation disarms |
| **Specific number** | "Forty quid and it took ten minutes" | Concreteness is credible |
| **Direct question** | "Does anyone else's [X] do this?" | Invites identification |
| **Warning** | "Don't buy [category] until you check this" | Loss aversion |
| **Comparison** | "I tried three. Only one worked" | Implies effort and honesty |
| **Confession** | "I didn't think this would work" | Scepticism-first is persuasive |
| **In-action** | Already doing the thing, no setup | Motion holds the eye |
| **Time-bound** | "Three weeks in, here's what happened" | Implies real use — **only if true** |

**"Mid-sentence" and "in-action" are the two most reliable and the most under-used**, because the model's default is an establishing shot with a lead-in.

That last pattern carries a warning: **a time-bound claim from a synthetic presenter is fabricated experience.** Use it only for a real customer's testimonial, not a generated one. See `ugc-disclosure-compliance`.

## Prompting the hook

The hook has two halves — the visual and the line — and both have to start immediately.

```
HOOKS = [
  "She is already mid-sentence, gesturing toward the counter with "
  "the bottle in hand, no lead-in. Close, subject fills frame.",

  "Close-up on the stained worktop first, then her hand enters "
  "frame fast holding the bottle. Action underway at frame one.",

  "She holds the product up toward the camera, already talking, "
  "slight head shake as if disagreeing with something.",

  "Very close on her face, mid-sentence, slight incredulous "
  "expression. Subject fills frame entirely.",
]
```

**"No lead-in" and "action underway at frame one"** are the clauses that remove the establishing shot. Without them the model gives you a slow push-in, which is the weakest opening available. See `video-hook-first-3-seconds`.

## Test properly

- **One variable.** Only the hook changes. Seed, references, body, everything else fixed
- **Draft the variants**, enhance only the winners
- **Judge muted, on a phone**, before spending media budget
- **Measure three-second retention and hook rate**, which move faster and need less volume than conversion
- **Don't call a winner on a day of data**

Internal preference correlates poorly with performance here. Its job is removing the broken, not picking the winner. See `video-ab-testing-variants`.

## The library is the asset

This is the part people skip, and it's where the compounding value is. Log every hook with its result.

```
Pattern         Line / action                        3s-ret   Verdict
mid-sentence    "...and it just lifted straight off"   61%     ✓ winner
result-first    clean worktop, then "here's how"       58%     ✓ keep
problem         "coffee rings on oak aren't stains"    56%     ✓ keep
confession      "I didn't think this would work"       54%     keep
mistake         "I was scrubbing it. Wrong"            51%     retest
direct-question "does anyone else's oak do this?"      47%     drop
comparison      "I tried three cleaners"               44%     drop
slow-push-in    product on counter, camera pushes in   29%     ✗ the default
```

Two things that record gives you:

**Patterns rather than one-offs.** After three rounds you know that problem-statement and result-first work for this product and direct-question doesn't. That's reusable across every future campaign.

**Ammunition against the default.** The bottom row — the slow push-in the model produces unprompted — losing by 30 points is the argument for prompting deliberately.

`list_video_generations(status="completed")` is free and paginated — use it to reconstruct a batch rather than your memory.

## Refreshing a fatigued creative

Ad fatigue usually shows up as **hook fatigue first**. The body still works; the opening has been seen.

```
Proven body clip (already enhanced)
+ 4 fresh hooks from the library, generated as drafts
= a refreshed creative for a few credits
```

**Fresh hooks on a proven body is the cheapest way to extend a creative's life**, and it's far cheaper than developing a new concept. Keep the library stocked for exactly this.

## Don't

- **Don't render hooks inside a long clip** while iterating. Standalone 4-second drafts.
- **Don't accept the slow push-in.** Prompt "no lead-in" explicitly.
- **Don't change more than the hook** between variants.
- **Don't test hooks at full quality.**
- **Don't use a time-bound experience claim** from a synthetic presenter.
- **Don't discard the log.** The pattern library is the compounding asset.
- **Don't develop a new concept** when fresh hooks on a proven body will do.
- **Don't trust internal preference** as a predictor.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`video-hook-first-3-seconds`, `ugc-script-writing`, `video-ab-testing-variants`, `ugc-batch-testing`, `ugc-performance-iteration`, `ugc-ad-video-generation`
