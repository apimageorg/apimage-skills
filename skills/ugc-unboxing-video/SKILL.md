---
name: ugc-unboxing-video
description: Generate unboxing-format video where the packaging, the reveal and the first-touch moment carry the sell. Use whenever the user wants unboxing content, package reveal video, first-impressions creative, or is launching a physical product.
---

# Unboxing Video

Unboxing works because it stages anticipation and then resolves it. The format is almost entirely hands, packaging and pacing — which makes it one of the best fits for generation, because there's no face to keep consistent and no experience being claimed.

It also has the tightest accuracy requirement in the whole UGC set: the box has to be *your* box.

## Photograph the packaging. Don't generate it.

The non-negotiable rule.

Models produce plausible packaging — plausible logos, plausible type, plausible layout. Plausible is worthless here, because the entire point of an unboxing is that the viewer recognises what will arrive.

```
create_brand_asset(type="product", ...)   # real photos. Free
```

Shoot and save:

```
[ ] Closed box, front
[ ] Closed box, three-quarter
[ ] Lid partly lifted
[ ] Open box with contents in place
[ ] Contents removed and laid out
[ ] Any insert card, sleeve or printed detail
```

**Generated text on packaging is the single most common giveaway** in AI unboxing content, and it's also a straightforward misrepresentation of what the customer receives. See `packaging-mockup-generation` and `product-image-qa-review`.

## The beat structure

```
0-3s     ARRIVAL      the sealed box. Hands already on it
3-6s     OPENING      cutting tape, lifting the lid
6-10s    REVEAL       the contents, first clear look
10-14s   FIRST TOUCH  picking it up, turning it, scale visible
14-17s   IN USE       one quick real-world moment
17-18s   CTA
```

**The reveal is the payoff and it needs a beat of stillness.** The most common pacing error is rushing past the moment the lid comes off — that frame is what the whole first ten seconds was building to.

Generate these as separate short clips against a fixed look and cut them. Identity drift isn't a concern; continuity of hands, surface and light is. See `multi-scene-video-assembly`.

## Prompting the beats

```python
PROD = ["<box front>", "<box three-quarter>", "<open box>", "<contents>"]
LOOK = ("Handheld phone footage, slight shake, filmed from above at "
        "a slight angle. Natural window light from the left, no fill. "
        "Ordinary wooden table, a phone and keys visible at the edge. "
        "Unpolished, shot on a phone. Not studio, not commercial.")

BEATS = [
  ("Two hands slide a box cutter along the tape seam of the sealed "
   "box, then pull the flaps open. Motion is slow and deliberate.", 5),

  ("Hands lift the lid straight up and set it aside, revealing the "
   "contents. Brief pause with the box open and still.", 5),

  ("A hand lifts the product out of the box and turns it slowly, "
   "showing one full rotation. Fingers visible for scale.", 6),
]
```

Two clauses doing specific work:

**"Fingers visible for scale"** — unboxing is where scale confusion is most costly, because the viewer is forming an expectation of what turns up. See `product-scale-reference`.

**"A phone and keys visible at the edge"** — an ordinary desk with ordinary clutter reads as real. A bare surface reads as a studio table. See `ugc-authenticity-signals`.

## Hands are the hardest part

Unboxing is a hands-heavy format and hands are the most common visual failure in generated video.

| Problem | Mitigation |
|---|---|
| Extra or fused fingers | Shorter clips, slower motion |
| Hands passing through the box | Simple, single-direction actions |
| Grip changing mid-clip | One action per clip, no re-grips |
| Fingers deforming at speed | "Slow and deliberate" in the prompt |
| Both hands interacting closely | Prefer one hand where possible |

**Check every frame where a hand meets the product.** Freeze the highest-motion moment, not a calm one. This is what the QA pass is for and it fails often enough that you should budget a re-render. See `product-image-qa-review`.

## Sound carries this format

Unboxing is unusually audio-driven — tape, cardboard, tissue, the click of a lid. If your pipeline separates audio, the tactile track is worth as much as the visual.

- Keep it dry and close. Reverb reads as a studio
- No music over the reveal. Let the sound of the box do it
- Music can come in after the reveal, not before

See `music-audio-pairing` and `ugc-asmr-sensory-format`.

## Don't promise what isn't in the box

The accuracy point again, in its commercial form. An unboxing sets a delivery expectation, and a mismatch is both a returns driver and a misleading-advertising problem.

```
[ ] Every item shown is actually included
[ ] Nothing shown that's sold separately, unless labelled
[ ] The packaging matches current production
[ ] Colours match the real product
[ ] Quantities match
[ ] No discontinued insert or old branding
```

**"Accessory sold separately" belongs on screen**, not in the caption. Captions are collapsed, muted and skipped.

## The variant that outperforms

A straight unboxing is a weak hook — the sealed box is the least interesting frame in the video. Two fixes:

```
Result first:   0-2s  the product already in use, working
                2-4s  cut back to the sealed box
                4s+   the unboxing proper

Problem first:  0-3s  the problem this solves, stated
                3s+   the box arriving as the answer
```

Both give the reveal something to resolve. Generate the hook as a standalone 4-second draft and test it against the plain open. See `ugc-hook-library` and `video-hook-first-3-seconds`.

## Don't

- **Don't generate packaging or printed text.** Photograph it.
- **Don't rush the reveal.** It needs a beat of stillness.
- **Don't show anything not in the box.**
- **Don't skip the hands QA pass** at high-motion frames.
- **Don't open on the sealed box** with no hook.
- **Don't put music over the reveal.**
- **Don't hide "sold separately" in the caption.**
- **Don't show old packaging.** It's a delivery promise.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`packaging-mockup-generation`, `product-scale-reference`, `ugc-asmr-sensory-format`, `multi-scene-video-assembly`, `product-image-qa-review`, `ugc-haul-format`
