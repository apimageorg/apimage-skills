---
name: tiktok-video-generation
description: Generate video for TikTok specifically — native formatting, hook conventions, sound-off viewing, and what the platform's own rules require of AI content. Use whenever the user wants TikTok videos, TikTok Shop content, TikTok ads, or asks how to make generated video work on TikTok.
---

# TikTok Video Generation

TikTok punishes content that looks like an advert and rewards content that looks native. Generated video defaults to looking like an advert — clean, polished, centred, well-lit — which is exactly the aesthetic the feed skips.

Making generated video work on TikTok is mostly about deliberately removing polish.

## The format

```
aspect_ratio  "9:16"
resolution    "hd" / 720p for organic, 1080p for paid
duration      6-15s for most content, 4-6s for a hook test
```

**Organic at 720p, paid at 1080p.** TikTok recompresses aggressively and the difference between a 720p and 1080p source mostly doesn't survive it. Spend the difference on more variants. See `video-model-selection`.

Safe area matters more here than anywhere: the platform covers roughly the bottom quarter with the caption, username and buttons, plus a strip down the right. Keep everything essential in the middle 60% and out of the right 12%. See `aspect-ratio-strategy`.

## Native, not polished

The single most useful adjustment to a generated TikTok is making it look less produced.

| Reads as an ad | Reads as native |
|---|---|
| Studio lighting, seamless background | Real room, mixed light, visible clutter |
| Smooth gimbal camera move | Handheld, slight shake |
| Product centred on a plinth | Product held, in use, partially out of frame |
| Colour-graded and even | Slightly imperfect exposure |
| Logo in frame | No logo until the end, if at all |
| Voiceover in a studio | Direct-to-camera, room sound |

Prompt for the imperfection explicitly, because the model won't volunteer it:

```
"Handheld phone footage, slight camera shake, natural indoor lighting
from a window, ordinary kitchen background with some clutter visible,
unpolished, shot on a phone. Not studio, not commercial."
```

The negative framing at the end — *not studio, not commercial* — does real work. The model's prior is toward commercial polish.

## The first second

TikTok's retention curve is steeper than any other platform. The opening frame decides it.

- **Start mid-action.** No establishing shot, no lead-in
- **No logo in frame one.** It reads as an ad instantly
- **Face or motion in the opening frame.** Both are pre-attentive
- **Make sense muted.** Most feed viewing starts silent

Generate the hook as its own 4-second clip so you can test several cheaply against one body. Almost all short-form performance variance lives here. See `video-hook-first-3-seconds`.

## Sound-off first

Assume the first viewing is silent. That means:

- The visual has to carry the message without narration
- Any spoken claim needs an on-screen equivalent
- Captions are not optional, and they go in the **middle third**, not the bottom, where the platform's own caption sits
- Music choice matters for the sound-on viewing but can't be load-bearing

Don't render text into the video. Models render text badly and you'll want to change it per variant. Overlay it in editing. 

## The production loop

```
1. generate_image          the hero still, in 9:16, unpolished framing
2. create_brand_asset      save product and presenter for reuse
3. generate_video (draft)  4-6s hooks, several variants, locked seed
4. judge muted, on a phone
5. enhance_video_draft     the winners only
6. assemble + caption      in an editor, not in the generation
```

Everything expensive happens once, on something already chosen. See `draft-then-enhance-workflow`.

## TikTok Shop and product content

For commerce content the platform rewards demonstration over description.

- **Show the product in use**, held by hands, in a real setting — not on a white background
- **One clear benefit per clip.** Not a feature list
- **Image-to-video from the real product photo**, so the product stays accurate. See `image-to-video-animation`
- **Save the product as a brand asset** so twelve clips show the same product. See `character-consistency-video`
- **Price and offer in the caption**, not burned into the video, so it can change

## Disclosure and platform rules

Two obligations, and both are real rather than advisory.

**AI content labelling.** TikTok requires realistic AI-generated content to be disclosed, and it applies its own labelling to content it detects. Label it yourself — undisclosed synthetic content that's detected can have distribution limited.

**Synthetic testimonials.** An AI-generated person presented as a real customer giving a real review is a false endorsement. That's an advertising-standards problem in many jurisdictions independent of platform policy, and TikTok's own synthetic media rules prohibit using AI likenesses to endorse products as if real.

The workable position: generated presenters and generated scenes are fine and clearly labelled; generated *testimony attributed to a real person who doesn't exist* is not. 

Also: `safety_tolerance` (0-4) controls the moderation threshold on generation. A stricter setting rejects more prompts but produces fewer surprises in output you're about to put behind ad spend. See `video-brand-safety-moderation`.

## Don't

- **Don't render polished, studio-lit video** for organic TikTok.
- **Don't put a logo in the opening frame.**
- **Don't put captions at the very bottom.** The platform's caption is there.
- **Don't burn text into the render.** Overlay it.
- **Don't judge the clip with sound on, on a desktop.** Muted, on a phone.
- **Don't render organic at 1080p** by reflex.
- **Don't present a generated person as a real customer.**
- **Don't skip the AI disclosure.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`video-hook-first-3-seconds`, `aspect-ratio-strategy`, `ugc-tiktok-shop-content`
