---
name: ugc-asmr-sensory-format
description: Produce ASMR and sensory-satisfaction video where texture, sound and slow motion carry the whole piece with no presenter and no claims. Use whenever the user wants ASMR content, satisfying video, texture-led creative, or sensory product content for TikTok and Reels.
---

# ASMR and Sensory Format

The best fit for generated video in the entire UGC set: no face, no dialogue, no experience claimed, no identity to keep consistent. Just texture, motion and sound.

It also has the highest completion rates in short-form, and it's the format where "commercial polish" is least penalised — sensory content is allowed to look good, it just has to look *tactile*.

## Why this format suits generation

| Constraint elsewhere | Here |
|---|---|
| Face consistency across clips | No face |
| Lip sync at 3 credits per second | No dialogue |
| Fabricated personal experience | No claims made |
| Creator persona design | Not needed |
| Disclosure of a synthetic person | No person depicted |

What's left is the thing models are genuinely good at: material behaviour under slow, simple motion. Which means the cost per usable clip is the lowest of any UGC format, and the iteration loop is fast.

The AI-content label still applies to generated footage. The rest of the compliance surface mostly doesn't. See `ugc-disclosure-compliance`.

## What actually satisfies

Sensory content works on a small set of physical events. Pick one per clip.

```
Pouring, viscous       oil, honey, cream, paint
Pressing and rebound   foam, gel, dough, clay
Peeling                film, wax, backing paper, seals
Cutting clean          soap, blocks, layered material
Powder and granules    falling, being scooped, levelled
Fabric and pile        being brushed, smoothed, run over
Absorption             liquid disappearing into a surface
Squeezing              tube, pump, dropper
Crushing               foam, crystals, packaging
Slow reveal            a lid lifting, a film pulling away
```

**One event per clip.** Two competing textures in five seconds satisfies nobody. Sequence them across a multi-beat video instead. See `multi-scene-video-assembly`.

## Prompting for texture

Sensory prompting is unusual in that it's almost entirely about material and light, and barely about action.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[PRODUCT_ASSET],
  prompt="Extreme close-up. Thick amber oil is poured slowly from "
         "the bottle onto dark oak, spreading in a viscous bead "
         "that catches the light along its edge. Slow, continuous, "
         "unbroken motion. Hard raking light from the left across "
         "the grain so texture is exaggerated. Shallow depth of "
         "field, sharp on the leading edge of the liquid. Nothing "
         "else moves in frame.",
  aspect_ratio="9:16", resolution="4k", duration=5, seed=4271
)
```

The clauses that matter:

- **"Extreme close-up."** Sensory content lives closer than you think
- **"Hard raking light... across the grain."** Raking light is the single biggest texture lever. Soft even light flattens material
- **"Slow, continuous, unbroken motion."** Any cut or acceleration kills it
- **"Nothing else moves in frame."** Background motion breaks the trance
- **"Sharp on the leading edge."** Give the eye one thing to hold

**Raking light is the technique.** Everything else in the prompt is secondary to a low, hard side light. See `product-relighting` and `product-detail-macro`.

## Resolution and duration, unusually

This is the one format where the settings advice inverts.

- **Generate at the highest resolution available.** Texture is the product, and compression artefacts destroy it in a way they don't destroy a talking head
- **Longer clips work.** 8-10 seconds of continuous pour outperforms three cut shots
- **Enhance the winners.** Draft to find the motion, then enhance — sensory content shows quality difference more than any other format. See `draft-then-enhance-workflow`
- **Slow motion by prompt**, not by post-slowdown, which introduces smearing

Budget accordingly: fewer, longer, higher-quality clips rather than many short ones.

## Sound is half of it, and probably more

An ASMR video with the wrong audio fails regardless of the visual.

- **Dry and close.** Reverb reads as a room, which reads as staged
- **No music.** Or a very low bed under it. The texture is the track
- **Match the sound to the material** exactly. A pour that sounds like the wrong viscosity is more jarring than a visual flaw
- **No voiceover.** At all
- **Watch the levels.** Platform normalisation flattens quiet detail

If your pipeline handles audio separately, record it — real material sounds are trivially easy to capture and are better than anything synthesised. See `music-audio-pairing`.

## Where generated sensory footage fails

Be aware of what to check, because the failures here are specific.

| Problem | Mitigation |
|---|---|
| Liquid volume not conserved, appearing or vanishing | Shorter clips, slower pour |
| Viscosity looking wrong for the material | Name the viscosity explicitly |
| Motion stuttering or looping | Regenerate. Don't fix in post |
| Surface texture going plasticky | Raking light, and say "matte" |
| Bubbles or droplets behaving oddly | Simpler motion |
| The product label distorting in the flow | Keep the label out of the motion |

**Watch it at full size, twice, at full attention.** Sensory content is judged by people watching closely, and small physics errors that survive a thumbnail glance are obvious at full screen.

## Making it sell something

The commercial risk of this format is a beautiful clip that sells nothing.

```
0-8s     THE TEXTURE      the sensory event, uninterrupted
8-11s    THE CONTEXT      one beat showing what it's for
11-13s   THE PRODUCT      identifiable. One clear second of label
13-15s   ONE ACTION       quiet. Don't shout at the end
```

- **The label needs one clear, legible second.** Otherwise the video is an advert for the category
- **Don't interrupt the sensory beat** with text or a cut. Let it complete
- **Keep the CTA quiet.** A loud ending on a calm video is jarring and measurably hurts completion

## Don't

- **Don't put two textures** in one clip.
- **Don't use soft even light.** Raking light.
- **Don't add music over the texture.**
- **Don't add a voiceover.**
- **Don't cut during the sensory event.**
- **Don't slow it down in post.** Prompt the slowness.
- **Don't generate at draft quality for delivery.**
- **Don't forget the legible product second.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-detail-macro`, `product-relighting`, `music-audio-pairing`, `ugc-unboxing-video`, `food-beverage-photography`, `faceless-video-automation`
