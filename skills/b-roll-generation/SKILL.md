---
name: b-roll-generation
description: Generate supporting b-roll to cover cuts, add texture and carry voiceover, cheaply and in volume. Use whenever the user needs cutaway footage, filler shots, atmospheric clips, footage to cover a jump cut, or visuals to accompany narration.
---

# B-Roll Generation

B-roll is the cheapest useful thing to generate. Short, no faces, no product accuracy requirement, no narrative — and it solves a long list of editing problems that would otherwise need reshoots.

It's also where generated video is at its most reliable, because the failure modes that plague generated video (faces, hands, text, product labels) mostly don't apply.

## What b-roll solves

| Problem | B-roll fix |
|---|---|
| A visible jump cut in a talking clip | Cut away over the join |
| Voiceover with nothing to look at | Texture and detail shots under the narration |
| A clip that's 3 seconds too short | Extend with a cutaway |
| Two clips that don't cut together | A neutral shot between them |
| A weak transition | Cover it |
| Monotonous pacing | Vary shot scale with inserts |
| A lip-sync segment join | Cover the seam. See `lip-sync-spokesperson-video` |

That first row is the most common use. Segmented lip-sync clips have visible joins, and a two-second product cutaway makes the join disappear entirely.

## What generates reliably

Stick to these and the hit rate is high.

| Subject | Why it works |
|---|---|
| **Texture and material close-ups** | No faces, no hands, no structure to get wrong |
| **Steam, smoke, liquid, particles** | Organic motion, no fixed form to distort |
| **Fabric, paper, surfaces moving** | Same |
| **Light and shadow shifting** | Motion without objects |
| **Hands, partially in frame, in motion** | Hands are hard — keep them moving and cropped |
| **Environments, wide, no people** | Wide shots hold up well |
| **Objects at rest with camera motion** | Camera does the work, subject stays still |
| **Out-of-focus background bokeh** | Almost impossible to get wrong |

What to avoid in b-roll: faces, legible text, crowds, complex hand interaction, and anything where a viewer would notice an error. B-roll is glanced at — but a warped hand in a cutaway is still a warped hand.

## Generate short and generate many

B-roll wants volume, not length.

```
Target:  2-3 seconds used in the edit
Generate: 4-5 seconds (trim margin)
Quantity: 8-12 clips per project
Model:    flux-3-video-draft — often good enough as-is
```

**B-roll is frequently fine at draft quality.** It's on screen for two seconds, often behind a caption, often out of focus. Enhancing every cutaway is wasted credits. Enhance the ones that carry a moment; leave the rest as drafts.

That's a meaningful cost difference across a project. See `video-credit-cost-management`.

## The call

```
generate_video(
  mode="text-to-video",
  model="flux-3-video-draft",
  prompt="Extreme close-up on linen fabric, soft shadow moving across "
         "the weave. Slow drift left. No people, no text. Soft "
         "directional daylight, shallow depth of field, neutral grade.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=4,
  seed=4271
)
```

Text-to-video is fine here — unlike product or presenter work, there's no specific subject to preserve. That makes b-roll the one place where text-to-video is the right default.

**"No people, no text"** is worth including. Models add incidental figures and signage, and a half-rendered background person in a cutaway is exactly the artefact that reads as cheap.

## Match it to the main footage

B-roll that doesn't match the primary footage draws attention to itself, which is the opposite of its job.

```python
LOOK = ("Soft directional daylight from the left, shallow depth of "
        "field, warm neutral grade, subtle handheld movement.")

BROLL = [
    "Extreme close-up on the fabric weave, shadow drifting.",
    "Steam rising from the cup, drifting right.",
    "Hands partially in frame, wiping the surface.",
    "Wide of the empty kitchen, morning light, no people.",
    "Out-of-focus foliage, bokeh, gentle movement.",
]

specs = [{
    "mode": "text-to-video",
    "model": "flux-3-video-draft",
    "prompt": f"{s} {LOOK} No people, no text.",
    "aspect_ratio": "9:16",
    "resolution": "hd",
    "duration": 4,
    "seed": 4271,
} for s in BROLL]
```

Same look clause verbatim, same seed, same aspect ratio. That's what makes a library of cutaways cut into one piece rather than five. See `batch-video-production`.

## Build a reusable library

The strongest argument for b-roll: it's not project-specific. A library of thirty generic cutaways in your brand's look serves every project afterwards.

```
create_brand_asset(type="background", ...)   # save the look references
```

Generate a batch once, per look. Textures, hands, environments, atmospherics, transitions. Then every subsequent edit has cutaways available at zero marginal cost.

That amortises well: the library costs one batch and saves a generation on every project after it.

## Editing with it

- **Cut away on motion**, not on a static frame. The cut disappears
- **2-3 seconds maximum** per cutaway in short-form. Longer and it stops being support and starts being the content
- **Vary the shot scale.** Close, wide, close. Three cutaways at the same scale feel static
- **Match motion direction** across the cut
- **Don't crossfade.** Hard cuts read as intentional
- **Out of focus is your friend** for anything behind a caption

See `multi-scene-video-assembly` and `video-caption-subtitle-planning`.

## Don't

- **Don't enhance every cutaway.** Draft quality is often fine for 2 seconds.
- **Don't put faces in b-roll.** All the failure modes, none of the payoff.
- **Don't include text or signage.** It garbles.
- **Don't omit "no people."** Stray figures are a common artefact.
- **Don't generate exactly the length you need.** Leave trim margin.
- **Don't use a different look clause** from the main footage.
- **Don't let a cutaway run past 3 seconds** in short-form.
- **Don't regenerate b-roll per project.** Build a library.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`multi-scene-video-assembly`, `batch-video-production`, `text-to-video-prompting`, `video-credit-cost-management`, `lip-sync-spokesperson-video`
