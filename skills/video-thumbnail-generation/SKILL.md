---
name: video-thumbnail-generation
description: Generate video thumbnails and cover images with generate_image, designed for the small size they are actually viewed at. Use whenever the user needs a thumbnail, a video cover image, a YouTube thumbnail, a social preview image, or has video with low click-through.
---

# Thumbnail Generation

A thumbnail is the highest-leverage single image in video, and it's almost always an afterthought — a frame grab from the clip, which is the worst option available.

Generate it deliberately with `generate_image`. It costs 1-9 credits, it's iterated in seconds, and on YouTube it's a larger determinant of views than the video itself.

## Where thumbnails actually matter

| Placement | Thumbnail weight |
|---|---|
| YouTube search and suggested | **Decisive** |
| YouTube channel page | **Decisive** |
| Shorts in suggested | High |
| Shorts, Reels, TikTok in-feed | Low — autoplay bypasses it |
| Instagram profile grid | High — it's the grid tile |
| Email and web embeds | Decisive |
| Shared links | Decisive |

**Autoplay feeds bypass the thumbnail**, which is why TikTok thumbnails matter less. Everywhere with a browse or search surface, they decide the click.

## Design for the size it's viewed at

The mistake that ruins most thumbnails: designing at full size and viewing at full size.

A YouTube thumbnail is often displayed around 210 × 118 pixels. On mobile suggested, smaller. A detailed composition that reads beautifully at 1280 × 720 is mud at that size.

**Design constraints that follow:**

- **One subject.** Not a scene. Not three elements
- **Fills the frame.** A small subject in a wide shot disappears
- **High contrast** between subject and background
- **3-5 words maximum** if there's text, and very large
- **No fine detail.** It's not visible
- **Strong, simple silhouette** readable at a glance

The test: shrink it to 200 pixels wide and look. If you can't tell what it is, it's failed — regardless of how good it looks large.

## The generation call

```
generate_image(
  model="flux-2-pro",
  prompt="Extreme close-up of a hand holding a small amber bottle, "
         "filling most of the frame. Deep teal background, strong "
         "contrast. Single subject, bold simple composition, high "
         "clarity. Dramatic side lighting. Empty space in the upper "
         "left third for text overlay.",
  aspect_ratio="16:9",
  seed=8812
)
```

Three things in there:

**"Filling most of the frame"** — the fix for the most common thumbnail failure.

**"Single subject, bold simple composition"** — prompting against the model's tendency toward detailed, busy scenes.

**"Empty space in the upper left third for text overlay"** — leave the text zone deliberately clear. Text over a busy area is unreadable at 200 pixels no matter what you do in the editor.

Iterate freely — image generation is the cheap operation. Lock the seed once the composition works and vary one element at a time. See `seed-locked-iteration`.

## Ratios

| Destination | Ratio |
|---|---|
| YouTube standard | **16:9** |
| YouTube Shorts cover | 9:16 |
| Instagram grid tile | **1:1** (crops from 9:16 — check it) |
| TikTok cover | 9:16 |
| Web embed / email | 16:9 |
| Podcast episode art | 1:1 |

For Instagram, remember the grid tile is a **1:1 crop of your 9:16 Reel cover**. Compose the subject centrally or the grid crops it badly. See `instagram-reels-generation`.

## Never use a frame grab

Worth stating plainly, because it's the default behaviour.

A frame from the video is optimised for being one frame in motion — mid-blink, mid-motion, mid-gesture, composed for a moving sequence rather than a still. It reads as accidental because it was.

A generated thumbnail can be composed for exactly the job: one subject, huge, high contrast, with room for text. That's a different image from anything in your clip.

## Text on thumbnails

Overlay it in an editor. Never render it in the generation — models produce garbled text, and you'll want to test variants anyway.

- **3-5 words.** "How to remove coffee stains" is too long; "COFFEE STAIN FIX" works
- **Very large.** Larger than feels right at full size
- **Heavy weight**, high contrast, with a stroke or shadow
- **Upper or lower third**, over the clear zone you prompted for
- **Don't duplicate the title.** The title is right next to it. Say something different
- **Leave the bottom-right clear** on YouTube — the duration badge sits there

That fourth point is the one people miss. A thumbnail repeating the title wastes the second message slot.

## Consistency across a channel

For a channel or a series, thumbnails should be recognisable as a set. Same treatment, varying subject.

```
seed=8812  + saved preset  + same composition rules  + same text style
```

Save the look as a brand asset preset so every future thumbnail starts from it:

```
create_brand_asset(type="preset", ...)
```

A viewer who recognises your thumbnails in a suggested column clicks more. That recognition is worth more than any individual thumbnail being clever. See `character-consistency-video`.

## Test variants

Thumbnails are cheap to generate and high-impact, which makes them the best value creative test available.

```
4-6 thumbnail variants, one variable each:
  - subject scale (close vs very close)
  - background colour
  - with text vs without
  - facial expression, if a face is present
```

YouTube offers thumbnail testing natively for eligible channels. Elsewhere, swap and measure click-through over comparable periods.

**Change one thing per variant** or you learn nothing reusable. See `video-ab-testing-variants`.

## Don't

- **Don't use a frame grab.**
- **Don't design at full size only.** Check it at 200 pixels wide.
- **Don't put multiple subjects in it.**
- **Don't render text in the generation.** Overlay it.
- **Don't duplicate the title** in the thumbnail text.
- **Don't put anything in the bottom-right** on YouTube.
- **Don't forget the Instagram 1:1 crop.**
- **Don't skip the clear text zone** at generation time.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`youtube-shorts-generation`, `instagram-reels-generation`, `aspect-ratio-strategy`, `video-ab-testing-variants`, `video-caption-subtitle-planning`, `seed-locked-iteration`
