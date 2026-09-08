---
name: video-to-video-restyling
description: Use video-to-video mode to restyle, upgrade or repurpose existing footage while keeping its motion. Use whenever the user has existing video to transform, wants to change the look of a clip, mentions video-to-video or restyling, or wants to reuse old footage in a new campaign.
---

# Video-to-Video Restyling

`mode="video-to-video"` takes existing footage and re-renders it in a new look while preserving the original motion. That makes it the only mode where the motion is a given rather than a gamble — which is a significant advantage, because motion is what generative video gets wrong.

## Why it's underused

Text-to-video and image-to-video both have to invent motion, and invented motion is where drift, morphing and unintended camera moves come from.

Video-to-video inherits real motion from real footage. If you have any usable footage at all — phone clips, old campaign assets, stock — restyling it is often more reliable than generating from scratch.

| | Generated from scratch | Restyled from footage |
|---|---|---|
| Motion quality | Invented, variable | **Real, inherited** |
| Camera behaviour | Often drifts | Exactly the original |
| Timing and pacing | The model's | Yours |
| Subject fidelity | Approximate | Follows the original |
| Setup needed | A prompt | Usable source footage |

## What it's good for

**Upgrading rough footage.** Phone footage with bad lighting or a dull grade, re-rendered with a considered look. The motion and the moment are real; the finish is generated.

**Repurposing old assets.** A campaign clip from two years ago, restyled to the current brand look. Cheaper than reshooting and the performance is already known.

**Consistency across mixed sources.** Footage shot by different people on different devices, restyled through one look so it cuts together. This is a genuinely hard problem in normal production and restyling handles it well.

**Format and mood variants.** The same moment as a warm daytime version and a moody evening version, for different placements or audiences.

**Stylisation** where a literal look isn't wanted.

## The call

```
generate_video(
  mode="video-to-video",
  model="seedance-2-0",
  reference_images=["<source video>"],
  prompt="Restyle: warm golden-hour light, shallow depth of field, "
         "film grain, muted teal shadows. Keep all motion, timing and "
         "framing exactly as in the source. Same subject, same action.",
  aspect_ratio="9:16",
  resolution="720p",
  duration=6,
  seed=4271
)
```

Two clauses do the work:

**"Keep all motion, timing and framing exactly as in the source."** State it explicitly. Without it the model takes liberties with the motion, which defeats the purpose of the mode.

**"Same subject, same action."** Guards against the model reinterpreting what's happening.

## Prompt for the look, not the content

Same principle as image-to-video: the source supplies the content. Describing it again competes with the source.

**Wrong — re-describes the scene:**
```
A woman in a kitchen pouring coffee into a white mug, morning light,
she smiles, warm and cosy, cinematic
```

**Right — describes only the treatment:**
```
Restyle: warm golden light, film grain, muted shadows, shallow depth
of field. Keep motion, timing and framing exactly. Same subject.
```

The second is shorter and holds the source better.

## Where it breaks

Be realistic about the limits — restyling is not magic and it inherits the source's problems.

- **Bad source motion stays bad.** Shaky, badly framed or poorly timed footage restyles into well-lit shaky footage
- **Faces drift** under heavy restyling. The stronger the style change, the more identity moves. For a recognisable person, keep the restyle light
- **Text in the source garbles.** Signage, packaging text and captions in the original will come back wrong
- **Product labels distort** in proportion to how far the style moves. For product work, restyle conservatively
- **Fast motion restyles worse** than slow motion
- **Extreme style changes** lose more of the source than modest ones

**The rule: the further the target look is from the source, the less of the source survives.** A grade and lighting change holds well. A photoreal-to-illustration transformation holds much less.

## Restyle conservatively for commercial work

For anything with a product or a recognisable person in it:

```
Conservative (holds up):
  "Restyle: warmer grade, softer light, subtle film grain.
   Keep everything else identical."

Aggressive (loses the source):
  "Restyle: anime illustration, flat colour, bold outlines."
```

Both are legitimate. The first is what you want for a product ad; the second is a creative choice that happens to discard product accuracy.

Check the output frame by frame at the high-motion points, same as any product video. Labels and faces go first. See `video-brand-safety-moderation`.

## Draft it

Restyling is exactly the kind of thing to draft first, because whether a look survives a particular source is not predictable.

```
1. Restyle a 4-second segment as a draft
2. Judge: did the motion hold? Did the subject survive?
3. Adjust the style prompt, same seed
4. Restyle the full clip once the look is right
```

See `draft-then-enhance-workflow` and `seed-locked-iteration`.

## Rights, which matter more here

Restyling uses source footage, and the source has rights attached.

- **Your own footage** — fine
- **Licensed stock** — check the licence. Many stock licences prohibit use as input to AI systems, and restyling is squarely that
- **Client footage** — confirm you have the right to modify and republish it
- **Anyone recognisable in the source** — the original release may not cover synthetic modification of their likeness
- **Third-party footage you didn't licence** — no

That second point catches people out the same way stock portraits do in `ai-avatar-presenter`. "I licensed the clip" and "I may use the clip as AI input" are different permissions, and a lot of stock licences now explicitly separate them.

## Don't

- **Don't expect restyling to fix bad motion.** It inherits it.
- **Don't re-describe the scene.** Describe the treatment.
- **Don't omit "keep motion and framing exactly."**
- **Don't restyle aggressively** over a product or a recognisable face.
- **Don't expect source text to survive.** It garbles.
- **Don't use licensed stock as input** without checking the licence covers AI use.
- **Don't restyle the full clip first.** Draft a segment.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-to-video-animation`, `text-to-video-prompting`, `draft-then-enhance-workflow`, `video-brand-safety-moderation`, `product-demo-video`
