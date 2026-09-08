---
name: product-demo-video
description: Generate product demonstration video that keeps the real product accurate while showing it in use. Use whenever the user wants a product video, a demo clip, a product in use, Shopify or ecommerce product video, or has a product photo they want turned into video.
---

# Product Demonstration Video

The core APImage use case: a product photo in, an ad-ready clip out. It works well and it fails in one specific way — the product stops looking like the product.

A demo clip where the label warps or the shape shifts is worse than no clip. Everything below is about keeping the product accurate while the scene moves around it.

## Always image-to-video

Text-to-video invents a product that resembles yours. For a demo that's disqualifying.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[PRODUCT_ASSET_1, PRODUCT_ASSET_2, PRODUCT_ASSET_3],
  prompt="Hands lift the bottle and tilt it, liquid moves inside. "
         "Camera locked. Soft window light from the left. "
         "The bottle and label stay exactly as shown. Kitchen counter.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

Three things in that call do the work:

**Real product photography as the reference**, not a generated approximation. If you generate a "similar" bottle and animate that, you're advertising a product that doesn't exist.

**Multiple references** where the model accepts them, covering the angles the motion will reveal.

**"Stays exactly as shown"** in the prompt. It won't fully prevent drift but it measurably reduces it.

Save the reference set once and reuse it across every clip in the campaign. See `character-consistency-video`.

## Motion that doesn't destroy the product

The single most useful rule: **the more the product moves, the more it warps.** Warping concentrates on labels, logos, text and edges — exactly the parts that have to be right.

| Safe | Risky | Avoid |
|---|---|---|
| Camera push in slowly | Product rotating to show the back | Product spinning fast |
| Camera pan across a static product | Hand rotating the product | Product tumbling |
| Liquid, steam, powder moving inside/around | Product being poured | Label facing away then back |
| Hand entering frame to pick it up | Product handed between people | Rapid cuts within the generation |
| Light and shadow shifting | Product being opened | Anything revealing an unseen face |
| Background moving, product still | Squeeze or deformation | Text needing to stay legible in motion |

**Motion that reveals what the reference doesn't show will be invented.** If your reference set is front-on only and the prompt rotates the product, the model makes up the back — and it will be wrong.

Either supply references covering the angles, or keep the motion within what the references support.

## The demo formats that work

Pick one per clip. Trying to do three in one generation produces a rushed mess.

**In-use.** Hands using the product for its actual purpose. The most persuasive and the most under-used, because generated video defaults to product-on-a-plinth.

```
"Hands already mid-application, cream spreading on skin, close-up.
Product visible in frame. Camera locked, handheld feel."
```

**Reveal.** Product enters frame, or is unveiled. Good as a hook beat, weak as a whole clip.

**Scale and context.** Product next to something familiar so size reads correctly. Underrated — size confusion is a real returns driver in ecommerce.

**Texture and detail.** Extreme close-up on material, finish, stitching, grain. Cheap, short, and it answers a question customers actually have.

**Mechanism.** How it opens, folds, attaches, adjusts. Highest-value for anything with a non-obvious function.

For before-and-after, see `before-after-transformation-video`.

## Structure a demo as beats

A 5-second clip shows one thing. A demo usually needs three or four, which means generating beats and cutting them.

```
Beat 1  2-3s  hook       hands already mid-use, close, fast
Beat 2  4s    context    product in the setting, wider
Beat 3  5s    mechanism  the thing it does, clearly
Beat 4  4s    result     the outcome
Beat 5  2s    CTA        product held to camera, lower frame clear
```

Same references, same look clause verbatim, same seed family, same aspect ratio. Then cut. See `multi-scene-video-assembly`.

## Unpolished beats polished

For organic social and UGC-style ads, the studio-lit product shot reads as an advert and gets skipped. Prompt against the model's default:

```
"Handheld phone footage, slight shake, ordinary kitchen counter with
some clutter, natural window light, unpolished. Not studio, not
commercial photography."
```

For a brand site or a paid placement where polish is appropriate, prompt the other way. The point is choosing rather than accepting the default. See `tiktok-video-generation`.

## Check the product frame by frame

The review step that catches what a casual watch misses.

```
[ ] Scrub through the highest-motion section frame by frame
[ ] Label legible and undistorted throughout
[ ] Logo shape correct — not stretched, not re-lettered
[ ] Product silhouette consistent across the clip
[ ] Colour matches the real product
[ ] No extra product, extra hand or invented object
[ ] Any on-pack text still reads correctly
```

**Warping is invisible at normal playback and obvious in a still.** Check stills from the fast parts, not the calm ones. `analyze_image` (1 credit) on a suspect frame will sometimes name an artefact you'd skimmed past.

If a label warps, the fix is less motion or better references — not a re-roll at the same settings. See `video-brand-safety-moderation`.

## Claim accuracy

Generated demo video is a product claim. It gets the same standard as a filmed one.

- **Don't show the product doing something it doesn't do.** A generated clip of a cleaner removing a stain instantly, when it doesn't, is a false claim however it was made
- **Don't imply a result the product can't deliver**
- **Don't show a configuration you don't sell**
- **Regulated categories** — health, beauty, supplements, financial — go through normal legal approval

Ease of production doesn't lower the substantiation bar. If anything it raises the discipline required.

## Cost

```
1. Real product photos          → your existing assets, free
2. edit_image / remove_background → clean up references if needed (2-3 credits)
3. create_brand_asset            → free, do it once
4. generate_video (draft)        → iterate motion cheaply
5. enhance_video_draft           → full quality on keepers only
```

See `draft-then-enhance-workflow` and `video-credit-cost-management`.

## Don't

- **Don't use text-to-video for a real product.**
- **Don't animate a generated approximation** of your product.
- **Don't rotate the product** unless the references cover the hidden faces.
- **Don't judge product accuracy at playback speed.** Check stills from the fast frames.
- **Don't put the product on a plinth** for organic social.
- **Don't cram three demo beats into one generation.**
- **Don't show the product doing something it doesn't do.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-to-video-animation`, `character-consistency-video`, `multi-scene-video-assembly`, `before-after-transformation-video`, `video-brand-safety-moderation`, `tiktok-video-generation`
