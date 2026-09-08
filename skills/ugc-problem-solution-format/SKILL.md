---
name: ugc-problem-solution-format
description: Build the problem-solution UGC structure, the highest-converting short-form format, by making the problem specific and visible before the product appears. Use whenever the user wants direct-response UGC, pain-point creative, or has an ad that explains the product before establishing why anyone cares.
---

# Problem, Then Solution

The most reliable direct-response structure in short-form, and the one most often built backwards. Brands open with the product because the product is what they want to talk about. The viewer has no reason to care yet, so they leave.

The problem comes first, it has to be *specific*, and it has to be **visible on screen**.

## The structure

```
0-3s     THE PROBLEM      shown, not described. Specific
3-6s     THE FAILED FIX   what people try that doesn't work
6-9s     THE MECHANISM    why it doesn't work. Earns trust
9-14s    THE SOLUTION     the product, demonstrated
14-17s   THE RESULT       same frame as the problem shot
17-18s   ONE ACTION
```

The two beats people cut are the failed fix and the mechanism. They're the ones doing the persuasive work: they explain why the viewer's current approach isn't working, which is what makes a new product feel necessary rather than optional.

## Specificity is everything

A generic problem addresses nobody.

| Generic | Specific |
|---|---|
| "Messy kitchen worktops" | "White ring where the coffee pot sits" |
| "Skin problems" | "Foundation catching in the lines round the nose" |
| "Disorganised workspace" | "The cable that falls behind the desk every time" |
| "Slow software" | "Fourteen tabs open to answer one question" |
| "Poor sleep" | "Awake at 3am, then fine at 6" |

**The test:** would someone with this exact problem stop scrolling because you named *their* thing? Generic problem statements get generic indifference. The specific version gets the recognition reaction, which is what a hook actually is.

Specificity costs you reach and buys you conversion. That is the correct trade in direct response.

## Show it. Don't say it.

The problem beat has to be visual, because 0-3s is watched muted.

```python
PROD = ["<product front>", "<product in hand>"]
LOOK = ("Handheld phone footage, slight shake, filmed close from "
        "just above the surface. Natural window light from the "
        "left, no fill. Ordinary oak worktop with visible use "
        "marks and a mug beside it. Unpolished, shot on a phone. "
        "Not studio, not commercial.")

BEATS = [
  # PROBLEM - no product in frame at all
  ("Very close on a pale ring mark on the wooden worktop. A "
   "fingertip traces the edge of it. Camera locked, no product "
   "visible.", 4),

  # FAILED FIX
  ("A hand scrubs the same mark hard with a wet cloth. The mark "
   "does not change. Camera locked on the same spot.", 4),

  # SOLUTION
  ("A hand applies from the bottle onto a cloth, presses it onto "
   "the mark, then buffs in small circles.", 6),

  # RESULT - identical framing to the problem beat
  ("Very close on the same patch of worktop, now unmarked. A "
   "fingertip traces where the ring was. Camera locked.", 4),
]
```

Two deliberate choices:

**No product in the problem beat.** The moment the bottle appears, it's an ad. Keep the first three seconds product-free and it stays a piece of content about a problem.

**Identical framing on the problem and result beats.** Same camera position, same crop, same light, same seed. That's what makes the change legible rather than asserted. See `before-after-transformation-video` and `seed-locked-iteration`.

## The mechanism beat is the underrated one

Explaining *why* the problem happens converts better than describing the solution, because it reframes the viewer's model of the situation.

```
"Coffee rings on oak aren't stains. The heat lifts the oil finish,
 so the wood underneath goes pale. Which is why scrubbing makes
 it worse — you're taking off more finish."
```

Then the product is obviously the right answer rather than a claimed one. And it's a substantiated statement about a mechanism rather than a performance claim, which is the safer kind of thing to say.

**One mechanism, one sentence.** Two explanations is a lecture.

## Don't manufacture the problem

The line where this format goes wrong.

| Acceptable | Not |
|---|---|
| Naming a real problem specifically | Inventing a problem that doesn't exist |
| Showing a genuine failure mode | Staging an exaggerated disaster |
| "This is why that happens" | Implying harm with no basis |
| A real limitation of alternatives | Misrepresenting how alternatives work |

Two specific traps:

**Health and safety framing.** Implying a product prevents harm — mould, bacteria, damage, illness — is a claim needing evidence, and in many markets it moves the ad into a regulated category. Don't reach for fear as the problem beat.

**Exaggerated demonstration.** A staged failure worse than anything real is a misleading depiction, even where every word said is true. This matters more with generated footage, because you can produce an exaggerated failure trivially. See `video-brand-safety-moderation`.

## The result must be achievable

The result beat sets the expectation the customer will judge you against.

- **Show a typical result**, not the best one you could render
- If the real result takes two coats and a day, **don't show one wipe**
- **Same conditions** as the problem beat. Different light is a cheat
- **Label anything unusual.** "Two coats, dried overnight" on screen

**Over-promising in the result beat is the main returns driver for this format.** A modest result that turns out to be accurate outperforms a spectacular one that doesn't, once you count refunds.

## Variants worth testing

```
Problem-first     the structure above                         baseline
Result-first      0-2s the fixed result, then "here's what it was"
Failed-fix-first  0-3s open on the thing that doesn't work
Mechanism-first   0-3s "coffee rings aren't stains"           counter-intuitive
```

All four use the same body beats. Only the opening changes, so generate four 4-second hook drafts against the same references and seed, then cut each onto one enhanced body. See `ugc-hook-library` and `draft-then-enhance-workflow`.

## Don't

- **Don't open with the product.** Problem first, product-free.
- **Don't use a generic problem.** Specific enough to be recognised.
- **Don't describe the problem.** Show it.
- **Don't cut the failed fix or the mechanism.** They persuade.
- **Don't change framing between problem and result.**
- **Don't invent or exaggerate the problem.**
- **Don't use fear or health claims** as the problem beat.
- **Don't show an atypical result** without labelling the conditions.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-ad-video-generation`, `before-after-transformation-video`, `ugc-hook-library`, `ugc-tutorial-demo`, `ugc-script-writing`, `video-brand-safety-moderation`
