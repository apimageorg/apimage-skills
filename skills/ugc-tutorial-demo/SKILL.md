---
name: ugc-tutorial-demo
description: Generate how-to and tutorial-format UGC where teaching something useful carries the product. Use whenever the user wants tutorial video, how-to content, demo creative, educational short-form, or a product video that has to earn the watch.
---

# Tutorial and Demo UGC

The tutorial is the format with the best watch-through and the longest shelf life, because it gives the viewer something before it asks for anything. It's also the format where a generated presenter is on the firmest ground: teaching a method requires no personal experience, so there's nothing to fabricate.

The trap is making it an ad in tutorial clothing.

## Teach the method, not the product

The test: **is the video still useful if you cut the product out?** If not, it's a demo pretending to be a tutorial, and the audience reads that immediately.

```
Weak:   "How to clean your worktop with [Product]"
        one step, and the step is "buy this"

Strong: "Why coffee rings come back, and the three finishes
         that need different treatment"
        genuinely useful. The product appears where it belongs
```

The strong version sells better *and* survives being saved and shared, which is where the compounding comes from.

**Give away the actual method.** Withholding the useful part to drive a click is the thing that makes the format stop working.

## The structure

```
0-3s     THE PROMISE     what they'll know in 20 seconds. Specific
3-6s     WHY IT FAILS    the common mistake. Earns the rest
6-14s    THE STEPS       two or three. Demonstrated, not described
14-17s   THE RESULT      shown, in the same frame as the before
17-18s   ONE ACTION
```

**Two or three steps. Never five.** Short-form tutorials fail on step count long before they fail on anything else, because the viewer leaves at the point where it stops feeling achievable.

Front-load specificity in the promise:

```
Weak:   "Here's how to look after wooden worktops"
Strong: "Coffee rings on oak, gone in ten seconds, without sanding"
```

## Demonstrate, don't narrate

The whole advantage of video over a written guide is that you can *show* the step. Generation is good at exactly this: hands doing a thing to an object.

```python
PROD = ["<product front>", "<product in hand>"]
LOOK = ("Handheld phone footage, filmed from just above the "
        "surface, slight shake. Natural window light from the "
        "left, no fill. Ordinary oak worktop with visible use "
        "marks. Unpolished, shot on a phone. Not studio.")

STEPS = [
  ("A hand wipes the surface clean with a dry cloth in two "
   "passes. Simple, clear motion.", 4),

  ("A hand applies a small amount from the bottle onto a cloth, "
   "then presses it onto the ring mark and holds.", 5),

  ("A hand buffs the mark in small circles, and the ring "
   "gradually disappears. Camera stays locked on the same spot.", 6),
]
```

**"Camera stays locked on the same spot"** is the clause that makes the step readable. A drifting camera during a demonstration destroys the before-and-after relationship the clip depends on.

## The before and after must share a frame

The credibility mechanism of any demo. Two separate clips shot from different angles are unfalsifiable and read that way.

- **Same camera position** for before and after
- **Same light**
- **Same crop**
- Ideally **one continuous take** where the change happens on screen
- If cut, the cut should be visible and honest, not a hidden replacement

Lock the seed and the look clause across the pair. See `before-after-transformation-video` and `seed-locked-iteration`.

## Show the failure mode

The step that separates a real tutorial from a demo: show what happens when you do it wrong.

```
"If you scrub it, you'll take the finish off, and that's why the
 mark comes back darker."
```

Three things at once: it's genuinely useful, it explains why the product is needed without claiming anything, and it's a reservation, which is the most persuasive thing a UGC script contains. See `ugc-script-writing`.

## What a generated presenter can teach

The identity question resolves cleanly here, which is why this format is worth reaching for.

| Can say | Cannot say |
|---|---|
| "Here's the method" | "This is how I do it" |
| "This works because [mechanism]" | "I've been doing this for years" |
| "Don't do X, it damages the finish" | "I ruined a table learning this" |
| "It's designed for oil-finished wood" | "It worked on mine" |

**Method is impersonal, so the format doesn't need fabricated experience.** Write it as instruction rather than recollection and the compliance question mostly disappears. See `ugc-disclosure-compliance`.

## Text on screen, always

Tutorials are watched muted more than any other format, and steps are the thing people scrub back to.

```
Step 1  Wipe dry
Step 2  Apply to cloth, press, hold 10s
Step 3  Buff in circles
```

Numbered, short, on screen for the whole step. And leave the safe margins clear, since platform UI covers roughly the bottom fifth and the right edge. See `video-caption-subtitle-planning`.

## Where this format pays off twice

A tutorial has a second life that ad creative doesn't:

- **Saves and sends**, which most platforms weight heavily
- **Search traffic** on the platform and on YouTube
- **Support deflection**, because it answers the question your inbox keeps getting
- **Long shelf life.** A method video doesn't fatigue the way a hook does

Which means the honest version of the calculation is that the tutorial is worth making even where it converts worse per view.

## Don't

- **Don't withhold the method.** Give it away.
- **Don't use more than three steps.**
- **Don't narrate what you could show.**
- **Don't split before and after across different angles.**
- **Don't skip the failure mode.** It's the useful part.
- **Don't rely on voiceover alone.** Steps on screen.
- **Don't let the presenter claim personal practice.** Teach the method.
- **Don't dress an ad as a tutorial.** The audience can tell.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-demo-video`, `before-after-transformation-video`, `ugc-problem-solution-format`, `video-caption-subtitle-planning`, `ugc-script-writing`, `youtube-shorts-generation`
