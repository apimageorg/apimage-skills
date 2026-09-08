---
name: ugc-day-in-the-life
description: Build day-in-the-life and routine-format UGC where the product appears inside a plausible day rather than being pitched. Use whenever the user wants lifestyle-integrated video, routine content, "a day with" creative, or wants a product shown in natural context across a sequence.
---

# Day in the Life

The softest-selling UGC format: the product appears where it would actually appear, in a day that reads as somebody's. It converts less directly than problem-solution and it does something that format can't — it shows *when and where* the product fits, which is often the real barrier to purchase.

It's also a multi-scene build, so it lives or dies on continuity.

## Why it works

The viewer isn't being told the product is good. They're being shown a life where it's ordinary. For products whose problem is "I don't know when I'd use this," that's more persuasive than any claim.

Best fit: routine products, consumables, anything with a habit attached, and anything where the barrier is imagination rather than doubt.

Poor fit: one-off purchases, considered high-ticket items, anything needing a spec comparison.

## Structure: time is the spine

```
0-3s     ANCHOR      a specific time and one concrete detail
3-7s     MORNING     the product's first appearance. In passing
7-11s    MIDDAY      unrelated. Keeps the day credible
11-15s   AFTERNOON   the product again, different context
15-18s   EVENING     close. Small, ordinary
```

**Include beats where the product doesn't appear.** A "day" in which every moment involves the product is an ad with timestamps. The unrelated beats are what make the product beats read as incidental — which is the entire mechanism.

Rule of thumb: **the product in no more than half the beats.**

## Anchor it in specifics

```
Weak:   "A day in my life"
Strong: "6:40. The kettle's louder than the baby, so this is
         the only quiet bit."
```

Time stamps, a named object, an actual constraint. Specificity is what separates a day from a montage.

## Producing the sequence

The technical challenge is that these beats span different times, rooms and light, while remaining one person's one day.

```python
CREATOR = ["<char ref 1>", "<char ref 2>", "<char ref 3>"]
PROD    = "<product asset>"
BASE    = ("Handheld phone footage, slight shake, slightly uneven "
           "exposure. Unpolished, shot on a phone. Not studio, "
           "not commercial.")

BEATS = [
  ("She fills the kettle at the sink, still half asleep. " + BASE +
   " Early morning light, cold and blue, ordinary kitchen with "
   "last night's dishes still out.", 4),

  ("She applies the product at the bathroom mirror, quickly, not "
   "looking at the camera. " + BASE +
   " Early morning light, cold and blue, small bathroom.", 5),

  ("She types at a kitchen table with a laptop and a cold mug. " +
   BASE + " Flat midday light through the window, same kitchen.", 4),

  ("She puts the product back in a drawer without looking. " +
   BASE + " Warm low evening light, same kitchen, lamp on.", 4),
]
SEED = 4271
```

Three things holding it together:

**Same creator references and seed** across every beat, or you have four people's days. See `ugc-character-consistency`.

**Light describes the time.** Cold blue for early, flat for midday, warm and low for evening. That progression is what makes it read as one day rather than four clips.

**Same rooms recur.** The kitchen appears at three different times. Save it as a background asset so it's the same kitchen. Free.

## Continuity is where this format fails

| Break | Fix |
|---|---|
| Different wardrobe in each beat | State the wardrobe in every prompt |
| Light not progressing | Describe time-of-day light explicitly |
| Different kitchen each time | Background brand asset |
| Face drifting | Fixed reference set and seed |
| Same light morning and evening | The tell that it's four clips |
| Hair or makeup changing | State it, or accept a single mid-day look |

**Wardrobe is the one people miss.** Unless the day deliberately includes changing, the top has to be the same in every beat, and the model will not do that on its own.

Run the side-by-side stills check across the whole sequence before assembling. See `multi-scene-video-assembly`.

## The product beat has to be underplayed

The failure mode is a mini-ad inserted into a day: the presenter turns to camera, holds the product label-out and explains it. That breaks the format instantly.

```
Wrong: "And this is the part of my routine I couldn't live
        without — [holds product to camera, label forward]"

Right: [applies it in about two seconds, at the mirror, not
        looking at the camera, already moving on]
```

**Don't present the product to the camera. Use it and move.** Label legibility for one clear second somewhere in the video is enough for recognition; you don't need it in every beat. See `ugc-authenticity-signals`.

## What this format cannot claim

Day-in-the-life is structurally an experience claim: *this is my routine*. A generated presenter has no routine.

So the workable versions are:

- **A disclosed brand day.** "A day at [Brand]" with a disclosed presenter, framed as illustrative
- **An illustrative use-case day**, labelled as such: "How people use this through a day"
- **A real customer's day**, filmed by them, with generated b-roll filling gaps
- **Product-only, no presenter.** Hands, objects and rooms across a day. No identity, no claim, and the cheapest to produce

**The product-only version is the one to reach for.** It keeps everything that makes the format work — context, timing, ordinariness — with no fabricated person and no lip sync cost. See `ugc-disclosure-compliance` and `b-roll-generation`.

## Don't

- **Don't put the product in every beat.** Half at most.
- **Don't open with a generic "day in my life".** Anchor it.
- **Don't keep the light the same** across times of day.
- **Don't change wardrobe** between beats.
- **Don't present the product to camera.** Use it in passing.
- **Don't let a generated presenter claim it's their routine.**
- **Don't skip the continuity pass** on stills.
- **Don't use this format for high-ticket considered purchases.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`multi-scene-video-assembly`, `ugc-character-consistency`, `lifestyle-product-photography`, `ugc-get-ready-with-me`, `ugc-authenticity-signals`, `ugc-lifestyle-photos`
