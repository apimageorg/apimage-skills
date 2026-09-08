---
name: ai-avatar-presenter
description: Design and build a reusable synthetic presenter — generate the face, save it as a brand asset, and keep it consistent across every clip. Use whenever the user wants an AI avatar, a virtual spokesperson, a recurring on-camera host, or needs a face they can legally use in ads.
---

# Building an AI Presenter

A presenter you own solves two problems at once: you don't need permission for anyone's likeness, and you get a recurring face that makes a channel or campaign feel like one thing.

The work is almost entirely in the setup. Get the face and the reference set right once, save it, and every future clip is cheap.

## Why generate rather than license

| | Real person | Stock portrait | Generated presenter |
|---|---|---|---|
| Likeness permission | Contract needed | Licence often **excludes** synthetic use | Yours |
| Consistency across clips | Reshoot needed | One image only | Unlimited |
| Cost per new clip | Day rate | New licence | Credits |
| Can be used in ads | With a release | Check the licence carefully | Yes |
| Risk of appearing elsewhere | Low | **High** — others licensed it too | None |

**Stock portraits are the trap.** Most stock licences either prohibit use as AI training/reference input or prohibit depicting the model as endorsing a product. Using a stock face as a lip-sync reference is very often a licence breach, and it's the mistake people make because the image is technically "licensed".

Generate instead. It's cheaper, unambiguous and reusable.

## Designing the presenter

Decide who they are before generating, because "a person" produces a generic result that reads as stock.

```
Age band        early 30s
Presentation    approachable, not glossy
Wardrobe        plain linen shirt, muted colour
Setting         home office / kitchen — matches the content
Energy          calm and direct, not high-energy pitch
```

**Match the presenter to the format.** A UGC-style ad wants someone who looks like a customer. A B2B explainer wants someone who looks like a colleague. A polished brand film wants a different person entirely. One presenter cannot do all three convincingly.

## Generate the face

```
generate_image(
  model="flux-2-pro",
  prompt="Portrait of a woman in her early thirties, short dark hair, "
         "plain grey linen shirt, neutral friendly expression, "
         "front-facing, both eyes visible, mouth closed. Soft even "
         "lighting from the front, no hard shadows on the face. "
         "Plain warm-white background, slight depth of field. "
         "Sharp focus on the eyes.",
  aspect_ratio="1:1",
  seed=8812
)
```

Iterate here — image generation is 1-9 credits and this is the cheap half of the job. Getting the face right at this stage costs a fraction of discovering it's wrong after a 90-credit lip-sync render. See `video-credit-cost-management`.

**The requirements that matter for downstream use:**

- Front-facing or near-front, head and shoulders
- Both eyes visible, mouth closed or barely open
- Soft even light, **no hard shadow across the lower face**
- Sharp on the face specifically
- Plain, uncluttered background
- No glasses glare, no hair across the mouth

Those exist because a lip-sync render inherits every problem in the source. A hard shadow on the mouth produces mushy mouth shapes at 3 credits per second. See `lip-sync-spokesperson-video`.

## Build a reference set, not one image

A single portrait gives the model one view. Three or four angles give it a face it can hold through motion.

```
seed=8812  "...front-facing..."                  → base
seed=8812  "...turned slightly left, 20 degrees..."
seed=8812  "...turned slightly right, 20 degrees..."
seed=8812  "...slight smile, front-facing..."
```

Hold the seed, change only the angle or expression. That keeps it recognisably the same person while giving the model coverage.

Where a video model accepts 9-30 reference images, use more of them. See `character-consistency-video`.

## Save it as a brand asset

The step people skip, and then reconstruct badly three weeks later.

```
create_brand_asset(type="character", ...)
```

Or generate and save in one call:

```
generate_brand_asset(type="character", prompt="...", ...)
```

Brand asset operations are **free**. There's no reason not to save an approved face the moment you approve it.

Then retrieve it forever:

```
list_brand_assets(type="character")
get_brand_asset(type="character", id="...")
```

## Using the presenter

**For a talking clip:**

```
generate_lip_sync(
  face_image="<character asset reference>",
  audio="<clean audio under 30s>"
)
```

**For a non-speaking clip:**

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=["<asset ref 1>", "<asset ref 2>", "<asset ref 3>"],
  prompt="She holds the product up to camera, natural small movements. "
         "Camera locked. Kitchen background. Everything else still.",
  aspect_ratio="9:16",
  duration=5,
  seed=4271
)
```

**Never re-describe her appearance in the prompt.** The references handle identity; describing it again competes with them and causes drift. Describe the situation and the motion only.

## Where the illusion breaks

Be realistic about the limits, because these are what make an avatar read as fake.

- **Hard profile angles.** A face saved front-on drifts badly at 90 degrees
- **Long clips.** Identity holds better over 5-8 seconds than over 20. Segment instead
- **Fast motion.** Motion and identity trade against each other
- **Hands near the face.** Both are hard; together they compound
- **Extended eye contact without blinking**, which is the classic uncanny tell
- **Emotion the audio implies but the face doesn't show.** Mismatch reads as wrong even when neither part is

The practical answer to most of these is **shorter clips, assembled** — which also cuts better. See `multi-scene-video-assembly`.

## Disclosure, plainly

A generated presenter is fine. A generated presenter **presented as a real customer giving a real testimonial** is a false endorsement, and that's an advertising-standards problem in most markets rather than a stylistic choice. Platforms also increasingly require synthetic-media labelling and apply their own detection.

The workable line: use the presenter as a host, narrator or brand spokesperson, label it where required, and don't attribute experience they didn't have. See `video-brand-safety-moderation`.

## Cost

| Step | Cost |
|---|---|
| Face iterations | 1-9 credits each. Iterate freely |
| Reference set (3-4 angles) | 1-9 each |
| Saving as a brand asset | **free** |
| Non-speaking clip | Draft cheap, enhance the winner |
| Lip-sync clip | **3 credits/second**, 30s max |

The whole setup costs a handful of credits and pays back on the second clip.

## Don't

- **Don't use a stock portrait** as a lip-sync reference. Check the licence — it very often prohibits it.
- **Don't use a real person's face** without written permission.
- **Don't build the presenter from one image.** Use a reference set.
- **Don't forget to save it.** Brand asset operations are free.
- **Don't describe her appearance** in prompts that already carry references.
- **Don't push identity through 20 seconds of fast motion.**
- **Don't attribute a testimonial to a synthetic person.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lip-sync-spokesperson-video`, `character-consistency-video`, `video-brand-safety-moderation`, `image-to-video-animation`, `multi-scene-video-assembly`, `ugc-creator-persona-design`
