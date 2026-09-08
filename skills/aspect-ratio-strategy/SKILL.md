---
name: aspect-ratio-strategy
description: Pick the right aspect ratio per platform and design safe areas so nothing important gets cropped or covered by UI. Use whenever the user is generating video or images for social platforms, mentions aspect ratio or vertical or square format, or has content getting cropped or overlaid by platform UI.
---

# Aspect Ratio and Safe Areas

APImage supports **1:1, 16:9, 9:16, 4:3, 3:4, 21:9 and 9:21**. Picking the wrong one costs a re-render; ignoring safe areas costs you the part of the frame the platform covers with its own interface.

The second is the more common and more expensive mistake, because the output looks fine in a preview and wrong in the feed.

## Platform to ratio

| Destination | Ratio | Notes |
|---|---|---|
| TikTok | **9:16** | Full-screen vertical |
| Instagram Reels | **9:16** | Same |
| YouTube Shorts | **9:16** | Same |
| Instagram feed post | 4:5 (use 3:4) | 3:4 is the closest supported. Crops slightly |
| Instagram carousel | 1:1 or 3:4 | 1:1 is safest across a carousel |
| YouTube standard | **16:9** | Horizontal |
| Facebook feed | 1:1 or 3:4 | Square performs well |
| LinkedIn | 1:1 or 16:9 | Square gets more feed height |
| X / Twitter | 16:9 or 1:1 | |
| Pinterest | 3:4 or 9:16 | Tall wins |
| Website hero | 16:9 or 21:9 | 21:9 for a cinematic band |
| Email | 16:9 or 1:1 | Keep it short in the vertical |

**Generate native, don't crop.** A 9:16 clip cropped from a 16:9 render loses two thirds of the frame and the composition was never designed for it. Generating at the target ratio costs the same and produces a composition that works.

## The vertical safe area

This is the part people miss. On a 9:16 feed video, the platform's own UI covers a substantial band at the top and bottom, and a strip down the right side.

```
┌─────────────────────┐
│  ~10% top           │  ← account name, sound, sometimes a status bar
├─────────────────────┤
│                     │
│                     │
│    SAFE ZONE        │  ← put everything that matters here
│    ~60% of height   │
│                     │
│                     │
├─────────────────────┤
│  ~25-30% bottom     │  ← caption, username, CTA, music, buttons
└─────────────────────┘
                    ↑
              right ~12%: like, comment, share, profile
```

Rough working numbers: **keep essential content in the middle 60% vertically and out of the right 12%.** Platforms differ and change their UI, so treat that as a conservative envelope rather than a spec.

What that means in practice:

- **Product, face and any text must sit centre-frame**, not near an edge
- **Never put a logo bottom-right.** It's under the share button
- **Never put a caption at the very bottom.** The platform's own caption goes there
- **Subtitles belong in the middle third**, not at the bottom as they would be in film
- **Leave the top clear** of anything you need read

## Composing for the safe area

The framing instinct from horizontal video actively hurts here. Vertical wants a different composition.

```
Prompt for vertical:
  "...subject centred and filling the middle of the frame, generous
   headroom above, empty space at the bottom of the frame, vertical
   composition..."
```

**"Empty space at the bottom" is a deliberate instruction**, not wasted frame. That space is where the platform's caption and buttons land, and designing it empty is what stops your content being covered.

If you're overlaying text in editing, prompt for a low-detail region where the text will sit. Text over a busy area is unreadable regardless of the font.

## One shoot, several ratios

For a campaign running across platforms, generate each ratio natively rather than cropping one master.

```
9:16   TikTok, Reels, Shorts       — the primary for short-form
1:1    Feed posts, carousels       — safest cross-platform
16:9   YouTube, website, email     — horizontal contexts
```

With a locked seed and the same prompt, the three renders share a look while each being composed for its own frame:

```
seed=4271  aspect_ratio="9:16"  → vertical composition
seed=4271  aspect_ratio="1:1"   → square composition
seed=4271  aspect_ratio="16:9"  → horizontal composition
```

It isn't free, but it's cheaper than three separate creative processes and far better than three crops. See `seed-locked-iteration`.

## Resolution pairs with ratio

| Ratio | Video resolution | Reason |
|---|---|---|
| 9:16 organic | 720p / `hd` | Platforms recompress hard |
| 9:16 paid | 1080p / `fhd` | Ad platforms are less forgiving |
| 16:9 YouTube | 1080p / `fhd` | Watched on larger screens |
| 1:1 feed | 720p / `hd` | Small display size |
| 21:9 web hero | 1080p / `fhd` | Wide and large |

**Organic vertical at 1080p is usually wasted.** Spend the difference on more variants. See `video-model-selection`.

## The call

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=["<vertical-composed still>"],
  prompt="Subject centred in the middle third, generous headroom, "
         "lower third kept clear and low-detail. Camera locked. "
         "Vertical composition.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

**The source still must already be the target ratio.** Animating a 16:9 still at `aspect_ratio="9:16"` forces the model to invent the sides or crop the middle, and both look wrong.

## Check it in situ

Preview in the actual app, not in a media player. A clip that looks well-composed at full size can have its subject entirely behind the caption block in a real feed.

Cheap check: screenshot a real post in the target app, drop your frame behind the UI, and look.

## Don't

- **Don't crop to reach a ratio.** Generate native.
- **Don't put anything important in the bottom third** of a vertical video.
- **Don't put a logo bottom-right.** It's under the buttons.
- **Don't put subtitles at the very bottom.**
- **Don't animate a 16:9 still into a 9:16 clip.**
- **Don't render organic vertical at 1080p** by reflex.
- **Don't judge composition in a media player.** Check it against the real UI.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`tiktok-video-generation`, `video-model-selection`, `image-to-video-animation`
