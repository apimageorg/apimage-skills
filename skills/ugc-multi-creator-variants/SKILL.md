---
name: ugc-multi-creator-variants
description: Run the same script across several creator personas to find which presenter resonates, the largest untested variable in most UGC programmes. Use whenever the user wants creator variants, is testing presenters, needs volume from one script, or has one creative that has stopped scaling.
---

# Multi-Creator Variants

Most UGC testing varies the script and holds the presenter constant. That's backwards: **who delivers the message is usually a larger performance variable than the message**, and it's the one commissioning real creators makes prohibitively expensive to test.

Generated creators invert that economics. Four personas, one script, four variants, one afternoon.

## Why the presenter is the bigger lever

- **Audience recognition** is the whole mechanism of UGC, and it's demographic before it's verbal
- **Ad fatigue is face-first.** The same presenter across a campaign burns out before the script does
- **Segments respond to different people.** One creative can't serve a 25-year-old and a 55-year-old
- **It's unpredictable.** Nobody, including people who are good at this, reliably picks the winner

The last point is the important one. If it were predictable you'd cast once. It isn't, so test.

## The test structure

One script. One product. One look. Four people.

```python
SCRIPT  = "<the same script, delivered by each>"
PROD    = "<product asset>"
LOOK    = ("Handheld phone footage, slight shake, slightly uneven "
           "exposure. Natural window light from the left, no fill. "
           "Unpolished, shot on a phone. Not studio, not commercial.")

CREATORS = {
  "A": (["<A front>", "<A 3/4 L>", "<A 3/4 R>"], "<A kitchen bg>"),
  "B": (["<B front>", "<B 3/4 L>", "<B 3/4 R>"], "<B flat bg>"),
  "C": (["<C front>", "<C 3/4 L>", "<C 3/4 R>"], "<C workshop bg>"),
  "D": (["<D front>", "<D 3/4 L>", "<D 3/4 R>"], "<D office bg>"),
}

for key, (refs, bg) in CREATORS.items():
    generate_video(
      mode="image-to-video",
      model="flux-3-video-draft",
      reference_images=refs + [PROD, bg],
      prompt=f"She is already mid-sentence, holding the bottle, "
             f"natural small head movements. {LOOK} "
             f"Everything else stays still.",
      aspect_ratio="9:16", resolution="hd", duration=6, seed=4271)
```

**Only the creator and their setting change.** Same seed, same look clause, same duration, same script. If two things vary you learn nothing.

Note the setting travels with the creator — persona and environment are one unit, and a workshop creator in a kitchen is a different persona. That's a deliberate exception to the one-variable rule, and it's the right call: you're testing *people*, not faces.

## Build the roster once

Four personas is a one-time cost, and all the asset operations are free.

```
Per creator:
  4-6 portrait images at a fixed seed, varying angle    a few credits each
  create_brand_asset(type="character", ...)             free
  create_brand_asset(type="background", ...)            free
  create_brand_asset(type="preset", ...)                free
```

Design them to differ on things that plausibly matter, not on looks:

```
A   late thirties, practical, kitchen, plain register
B   mid twenties, fast, small flat, informal
C   fifties, measured, workshop, understated
D   early thirties, brisk, office, direct
```

**Differentiate on age, energy, setting and register.** Four attractive 25-year-olds is not a roster. See `ugc-creator-persona-design` and `ugc-diversity-representation`.

## Adapt the script to the register

The one thing that shouldn't stay literally identical. A script written for one voice sounds wrong in another, and testing a bad delivery tells you nothing about the persona.

```
A:  "Coffee rings aren't stains. It's the finish lifting. This
     puts it back. Ten seconds, done."

B:  "Okay so I thought my table was ruined? It wasn't. It was the
     finish. This fixed it in like ten seconds."

C:  "I'd assumed the marks were permanent. They weren't — the
     finish had lifted, and this restored it."
```

Same information, same claims, same structure, same length. Different mouth. See `ugc-script-writing`.

**Keep the claims identical across variants.** If one version says something stronger, you've tested a claim rather than a creator — and you now have a claim in market that never went through review.

## Cost control

The reason this is worth doing is that it's cheap if sequenced properly.

```
1. Draft all four at 6s                    cheap
2. Kill the broken ones on sight           free
3. Enhance the two survivors               the real spend
4. Lip sync only the face beats            3 credits/second
5. Same voiceover over shared b-roll       one render, four uses
```

**The b-roll is shared.** Product shots, hands, the result — none of that contains a face, so one enhanced set serves all four variants. Only the face beats are per-creator, and only those need lip sync.

Lip sync is `seedance-2-0` only, 3 credits per second, 30-second cap. Four 12-second face beats is 144 credits; four full 18-second videos synced end to end is 216 for no benefit. See `lip-sync-spokesperson-video` and `draft-then-enhance-workflow`.

## Reading the result

- **Measure three-second retention and hook rate first.** They move fastest and need the least volume
- **Then conversion**, once there's enough data to trust
- **Segment the read.** The most useful finding is usually "C wins for over-45s, B wins for under-30s" — which is a targeting decision, not just a creative one
- **Run the winners in parallel** rather than consolidating. Different faces to different segments is more valuable than one champion
- **Don't call it in a day**
- **Judge muted on a phone** before spending media

`list_video_generations(status="completed")` is free and paginated — use it to reconstruct which variant was which. See `video-ab-testing-variants` and `ugc-performance-iteration`.

## Rotating the roster against fatigue

The recurring use for a roster, beyond the initial test. When a creative fatigues:

```
Same script, same body b-roll, fresh face   → a new creative for
                                              the cost of one face beat
```

Cheaper than a new concept and it addresses the actual cause, since face fatigue precedes message fatigue. Keep two unused personas in reserve for exactly this. See `ugc-hook-library`.

## Don't

- **Don't test scripts while holding the presenter fixed.** Test both.
- **Don't vary more than the creator** and their setting.
- **Don't build a roster of near-identical people.**
- **Don't deliver one script verbatim** across incompatible registers.
- **Don't let claims drift** between variants.
- **Don't re-render shared b-roll** per creator.
- **Don't lip sync non-face beats.**
- **Don't consolidate to one winner** if segments differ.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-creator-persona-design`, `video-ab-testing-variants`, `ugc-batch-testing`, `ugc-diversity-representation`, `ugc-character-consistency`, `ugc-performance-iteration`
