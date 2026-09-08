---
name: character-consistency-video
description: Keep the same character, product or background identical across many generations using APImage brand assets and reference images. Use whenever the user needs a recurring presenter, a consistent product across a campaign, a repeating background, or complains that the subject changes between generations.
---

# Character and Product Consistency

The default behaviour of generative video is that every clip invents a new subject. For a campaign that's fatal — a "spokesperson" whose face changes between clips isn't a spokesperson, and a product whose label shifts isn't your product.

APImage solves this with **brand assets**: reusable characters, products, backgrounds and presets that persist across generations.

## Seeds are not the answer

A common mistake worth naming. A locked seed gives you a consistent *visual treatment* — lighting, grade, rendering character. It does **not** give you a consistent *subject*.

```
seed=8812  "a woman with short dark hair, kitchen"   → person A
seed=8812  "a woman with short dark hair, outdoors"  → person B, similar vibe
```

Same treatment, different person. For identity you need reference images and saved assets. Seeds and assets solve different problems and you generally want both. See `seed-locked-iteration`.

## The brand asset workflow

```
1. Generate the subject           generate_image, iterate cheaply
2. Approve one                    the canonical reference
3. Save it                        create_brand_asset
4. Reuse it forever               list_brand_assets / get_brand_asset
```

The tools, all **free** except generation:

| Tool | Cost | Use |
|---|---|---|
| `create_brand_asset` | free | Save an approved result as a reusable asset |
| `list_brand_assets` | free | Browse by type — character, product, background, preset |
| `get_brand_asset` | free | Retrieve a specific one by type and ID |
| `update_brand_asset` | free | Patch fields on an existing asset |
| `delete_brand_asset` | free | Remove permanently |
| `generate_brand_asset` | generation credits | Generate **and** save in one call |

`generate_brand_asset` is the shortcut when you know you'll reuse the result — it saves a round trip.

## Building a presenter

The most common use, and the order matters.

```
1. generate_image
     "Portrait of a woman in her early 30s, short dark hair, light
      grey t-shirt, neutral expression, soft even lighting, plain
      warm-white background, front-facing, sharp focus on face"
     → iterate at 1-9 credits until the face is right

2. Generate 3-4 more angles at the same seed, varying only the angle
     → a reference set, not a single image

3. create_brand_asset(type="character", ...)
     → save the set

4. Every future clip references it
```

**A reference set beats a single reference.** One front-facing portrait gives the model one view to work from; three or four angles give it a face it can hold onto through motion. Where the model accepts 9-30 reference images for video, use them.

## Using it in generation

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[
    "<character asset ref 1>",
    "<character asset ref 2>",
    "<character asset ref 3>"
  ],
  prompt="She speaks to camera, natural head movement. Camera locked. "
         "Same lighting. Kitchen background.",
  aspect_ratio="9:16",
  duration=6,
  seed=4271
)
```

Note what the prompt does *not* do: describe her appearance. The references handle identity; re-describing it in the prompt competes with them and causes drift. **Describe the situation and the motion, never the subject.** Same principle as `image-to-video-animation`.

## Products

Same pattern, higher accuracy requirement, because a warped label is obviously wrong in a way a slightly-different face isn't.

- **Use real product photography** as the reference. Not a generated approximation
- **Several angles**, saved together as one asset
- **Keep motion modest.** Labels and logos distort first, and they distort where motion is greatest
- **Check the output at the high-motion frames.** Don't judge from a still

```
create_brand_asset(type="product", ...)
```

Then every product clip in the campaign starts from the same reference set, which is what makes twelve clips look like one campaign.

## Backgrounds and presets

The two underused asset types.

**Backgrounds** — a saved location means every clip in a series appears to happen in the same place. That's a strong continuity signal and it costs nothing once saved.

**Presets** — a saved look (lighting, grade, lens character) applied across a set. This is the asset type that makes a campaign feel authored rather than assembled.

For a series, saving all four types — character, product, background, preset — means a new clip is a new prompt against a fixed visual world.

## Consistency across a campaign

```
Character asset    the presenter                    → same person, 12 clips
Product asset      the product, 4 angles            → accurate every time
Background asset   the kitchen                      → same place
Preset             warm, soft, shallow depth        → same look
Locked seed        per clip family                  → same treatment
```

That combination is what separates a campaign from twelve unrelated clips. Each element is cheap; the discipline is remembering to save them at the point you approve them, rather than reconstructing later.

## Where consistency still breaks

Be realistic about the limits.

- **Extreme angle changes.** A face saved front-on will drift in a hard profile
- **Long clips.** Identity holds better over 5 seconds than over 20
- **Fast motion.** Motion and identity trade off against each other
- **Prompt fighting the reference.** Describing appearance in the prompt undermines the asset
- **Across models.** A reference set that works well on one model may hold less well on another

The practical answer to most of these is shorter clips, assembled. Five 6-second clips hold identity better than one 30-second clip, and they cut better anyway. See `multi-scene-video-assembly`.

## Don't

- **Don't rely on seeds for identity.** They give treatment, not subject.
- **Don't use a single reference image** where the model accepts a set.
- **Don't describe the subject's appearance** in a prompt that has references.
- **Don't use a generated approximation of a real product.**
- **Don't forget to save the approved result.** Reconstructing it later costs more than `create_brand_asset` did.
- **Don't expect identity to survive 30 seconds of fast motion.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-to-video-animation`, `lip-sync-spokesperson-video`, `seed-locked-iteration`, `multi-scene-video-assembly`, `ugc-character-consistency`
