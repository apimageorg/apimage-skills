---
name: image-to-video-animation
description: Animate a still image into video with reference_images, keeping the exact subject rather than letting the model invent one. Use whenever the user wants to turn a photo into video, animate a product shot, mentions image-to-video, or is getting the wrong subject out of text-to-video.
---

# Image-to-Video Animation

Text-to-video invents a subject. Image-to-video animates the subject you supply. For anything where the specific product, person or scene matters, image-to-video is the only mode that works.

It's also the core APImage workflow — "turn a product photo into a UGC-style ad video" is exactly this mode.

## Why it's the default for commercial work

| | text-to-video | image-to-video |
|---|---|---|
| Subject fidelity | Invented, approximate | **Yours, exactly** |
| Brand consistency | Poor | Good |
| Product accuracy | Unreliable | Reliable |
| Iteration | Whole clip changes | Only the motion changes |
| Setup | A prompt | A prompt plus a good still |

The trade is that you need a good still first — which is cheap. Image generations cost 1-9 credits; video costs a great deal more. **Getting the still right is the cheap half of the job.**

## The still decides the outcome

A mediocre still animates into a mediocre video, and no amount of prompt work fixes it.

**What makes a good source still:**
- Sharp, well-lit, correct aspect ratio for the target platform
- Subject clearly separated from the background
- Composition with room for the motion you're about to add
- No motion blur
- Nothing important at the extreme frame edges, which move most

**Leave room for the motion.** If you want a push-in, the subject shouldn't already fill the frame. If you want the subject to move left, there should be space on the left. Framing the still tightly and then asking for movement produces cropping and warping.

Generate the still with `generate_image`, iterate there cheaply, then animate the approved one:

```
1. generate_image     1-9 credits, iterate freely
2. edit_image         relight, adjust angle, upscale if needed
3. generate_video     mode="image-to-video", the approved still as reference
```

## The call

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=["<the approved still>"],
  prompt="Camera slowly pushes in. Steam rises from the cup and drifts "
         "right. Everything else stays still. Soft window light "
         "unchanged. Calm pace.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

**`reference_images` accepts up to 4 for images and 9-30 for video, model-dependent.** More references give the model more to hold onto — useful for a product that must stay accurate from several angles.

## Prompt for motion only

This is the key difference from text-to-video prompting. The subject is already decided. Describing it again competes with the reference image and produces drift.

**Wrong — re-describes the subject:**
```
A white ceramic mug of coffee on a wooden table with steam rising,
soft morning light, cozy atmosphere, camera pushes in
```

**Right — describes only what changes:**
```
Camera pushes in slowly. Steam rises and drifts right. The mug and
table stay completely still. Lighting unchanged.
```

The second is shorter and works better. **State what moves, state what doesn't, and leave the subject alone.**

"Everything else stays still" is a genuinely useful clause. Without it the model tends to animate things you didn't mean — background elements shifting, the table subtly warping.

## Motion that suits a still

Not every motion is achievable from a single frame. The model has to invent everything it can't see.

| Reliable | Risky |
|---|---|
| Camera push in / pull out | A person turning to reveal their other side |
| Pan and tilt across the scene | An object rotating to show a hidden face |
| Parallax on a layered scene | Anything requiring occluded detail |
| Steam, smoke, liquid, fabric movement | A hand appearing from off-frame holding something specific |
| Subtle subject shift or breathing | Large subject repositioning |
| Light change, shadow movement | Text remaining legible through motion |
| Particles, dust, bokeh drift | Complex hand interaction with the product |

**The rule: motion that reveals what the still doesn't contain will be invented, and invention is where warping comes from.** Keep the motion within what a camera could do to that frame.

## Product accuracy

For commercial product video this is the whole point, and it needs care.

- **Use the real product photo** as the reference, not a generated approximation
- **Multiple references** of the same product from different angles, where the model supports it
- **Save the product as a brand asset** so every future clip uses the same reference set
- **Keep motion modest.** Big moves distort labels, logos and shapes
- **Check the output frame by frame** at the points of most motion. Labels warp first

```
create_brand_asset(type="product", ...)   → save the approved reference
list_brand_assets(type="product")         → reuse it across every clip
```

See `character-consistency-video`.

## The efficient production loop

```
1. generate_image        → hero still, iterate cheap (1-9 credits)
2. create_brand_asset    → save it, free
3. generate_video (draft) → animate, iterate motion cheap
4. enhance_video_draft    → full quality, once
```

Every expensive step happens once. See `draft-then-enhance-workflow`.

## Don't

- **Don't re-describe the subject in the prompt.** Describe the motion.
- **Don't frame the still tightly** and then ask for a push-in.
- **Don't ask for motion that reveals hidden detail.** It gets invented.
- **Don't animate a mediocre still.** Fix the still first — it's the cheap half.
- **Don't use a generated approximation** of a real product. Use the real photo.
- **Don't skip "everything else stays still".** It prevents a lot of unwanted motion.
- **Don't use big camera moves on a product with a label.** It warps.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`text-to-video-prompting`, `character-consistency-video`, `draft-then-enhance-workflow`
