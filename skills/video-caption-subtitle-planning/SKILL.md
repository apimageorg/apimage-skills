---
name: video-caption-subtitle-planning
description: Plan captions, subtitles and on-screen text for generated video — where they go, why they never belong in the render, and how to design frames that leave room for them. Use whenever the user is adding text to video, mentions captions or subtitles or on-screen text, or has text that came out garbled in a generation.
---

# Captions and On-Screen Text

Two rules, and the first one is absolute.

**Never render text inside the generation.** Generative models produce garbled, misspelled and warping text. Even when a single frame looks right, it deforms across motion. Overlay text in an editor.

**Second: design the frame for the text before you generate it.** Text over a busy area is unreadable regardless of the font, and by the time you're in the editor the frame is fixed.

## Why not in the render

| | Rendered text | Overlaid text |
|---|---|---|
| Legibility | Garbled, often misspelled | Perfect |
| Stability in motion | Warps and morphs | Fixed |
| Editable | Re-render required | Instant |
| Per-variant changes | New generation each | One project, many exports |
| Localisation | New generation per language | Swap the text layer |
| Accessibility | Not machine-readable | Real caption files |

The localisation point is the practical clincher. One clean visual plus twelve text layers gives you twelve markets. Twelve generations with burned-in text gives you twelve renders and twelve chances of a spelling error.

## Where text can go, per format

Vertical video is the constrained case, because the platform covers most of the frame with its own interface.

```
9:16 — 1080 × 1920
┌─────────────────────┐
│  0-10%   PLATFORM   │  account name, sound, status bar
├─────────────────────┤
│  10-20%  usable     │  ← headline text, sparingly
│                     │
│  30-60%  SAFE       │  ← subtitles belong HERE
│                     │
│  60-72%  usable     │  ← secondary text
├─────────────────────┤
│  72-100% PLATFORM   │  caption, username, CTA, buttons
└─────────────────────┘
             right 12%: like, comment, share
```

**Subtitles go in the middle third, not the bottom.** This is the counterintuitive one — film and television put subtitles at the bottom, and doing that on TikTok or Reels puts them behind the platform's own caption block. Middle-third placement is standard in short-form for exactly this reason.

| Format | Text zone |
|---|---|
| 9:16 vertical | Middle third. Never bottom 28%, never right 12% |
| 1:1 square | Centre, generous margins all round |
| 16:9 horizontal | Lower third is fine — no platform UI there |
| Stories | Middle, avoid top 15% and bottom 20% |

## Design the frame for it

This is the step that has to happen at generation time. Prompt for a low-detail region where the text will sit.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[still],
  prompt="Product centred in the upper-middle of frame. The lower "
         "third of the frame is plain, uncluttered and evenly lit — "
         "empty counter surface, no detail. Camera locked. Vertical "
         "composition.",
  aspect_ratio="9:16",
  duration=5,
  seed=4271
)
```

**"Plain, uncluttered and evenly lit" in the text zone** is the instruction that makes overlaid text readable. Without it you get an interesting, busy frame and text you then have to put a semi-transparent box behind — which looks like a compromise, because it is.

Same principle for images: leave the region empty deliberately. See `aspect-ratio-strategy`.

## Subtitles

Non-negotiable for short-form, because most feed viewing starts muted.

- **Every spoken word captioned.** Not a summary
- **1-4 words on screen at a time**, appearing in time with speech. Full-sentence blocks are the amateur tell
- **Large.** Much larger than film subtitles — this is watched on a phone at arm's length
- **High contrast** with a stroke or subtle shadow, not a background box
- **Middle third**, per above
- **Consistent position.** Jumping subtitles are exhausting

Word-by-word or short-phrase timing is what makes short-form captions feel native rather than like a foreign film.

## On-screen text beyond subtitles

| Type | Where | Timing |
|---|---|---|
| **Hook text** | Upper-middle, frame one | Appears immediately, holds 2-3s |
| **Claim / benefit** | Middle | With the beat it supports |
| **Price / offer** | Middle-lower usable band | Late, after the value |
| **CTA** | Middle, over a clear frame | Final 2-3s |
| **Product name** | Anywhere clear | Once, briefly |
| Logo | Small, corner — **not bottom-right** | End only |

**Bottom-right is under the share button.** Logos there are invisible on every vertical platform.

## Hook text and visual hook are different jobs

A common mistake: using the caption to carry a hook that the visual doesn't support.

If the visual is inert and the text is doing all the work, the visual is failing — and on a platform where people scroll on the image before reading, that loses. The text should reinforce a visual hook, not substitute for one. See `video-hook-first-3-seconds`.

## Accessibility, and the practical benefit

Real caption files (SRT/VTT) rather than burned-in text:

- Are machine-readable, which matters for platform indexing and search
- Can be toggled by the viewer
- Can be swapped per language without touching the video
- Meet accessibility expectations, and legal requirements in some contexts

Some platforms auto-generate captions. They're usually mediocre, and they're worth replacing with a corrected file rather than accepting.

Practical approach: **burned-in styled text for the hook and key beats** (visual, punchy, part of the design), **plus a real caption track** for the spoken content.

## Localisation

The strongest argument for keeping text out of the render.

```
1 clean visual generation
+ 12 text layers
= 12 market-ready videos, one render
```

Design the frame with the text zone empty and generously sized — German and Finnish run considerably longer than English, and a zone sized for the English copy overflows.

## Don't

- **Don't render text into the generation.** The one absolute rule here.
- **Don't put subtitles at the bottom** of a vertical video.
- **Don't put a logo bottom-right.**
- **Don't use full-sentence subtitle blocks.** 1-4 words, timed.
- **Don't put text over a busy area.** Design the zone at generation time.
- **Don't rely on a caption to carry a dead visual.**
- **Don't size the text zone for English only.**
- **Don't accept auto-generated captions** without correcting them.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`aspect-ratio-strategy`, `video-hook-first-3-seconds`, `tiktok-video-generation`, `multi-scene-video-assembly`, `video-thumbnail-generation`, `ugc-tutorial-demo`
