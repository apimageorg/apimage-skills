---
name: instagram-reels-generation
description: Generate video for Instagram Reels specifically — where it differs from TikTok, feed crossover, aesthetic expectations and safe areas. Use whenever the user wants Reels content, Instagram video, Instagram ads, or asks how generated video should differ between Instagram and TikTok.
---

# Instagram Reels Generation

Reels and TikTok share a format and reward different things. Treating them as one destination and posting the same file to both leaves performance on the table on whichever platform you optimised against.

## Where Reels differs from TikTok

| | TikTok | Reels |
|---|---|---|
| Aesthetic tolerance | Rewards rough, native, unpolished | Tolerates and often rewards **polish** |
| Audience | Discovery-led, low follower dependence | Mix of discovery and existing following |
| Feed crossover | None — vertical only | **Reels appear cropped in feed and grid** |
| Trend velocity | Very fast | Slower, more evergreen |
| Text density | High, native | Lower. Cleaner design expectations |
| Brand presence | Reads as intrusion | More accepted |
| Watch context | Full-screen, sound-on more common | Often muted, often mid-feed scroll |

**The polish difference is the actionable one.** A studio-lit, well-graded product clip that gets skipped on TikTok can perform well on Reels, where the platform's visual culture is more designed. You can prompt for quality here rather than prompting against it.

That doesn't mean corporate. It means *considered* — good light, clean composition, intentional grade — rather than deliberately rough.

```
For Reels:
  "Soft directional daylight, clean composition, shallow depth of
   field, warm neutral grade, subtle handheld movement."

For TikTok:
  "Handheld phone footage, slight shake, ordinary room with visible
   clutter, natural mixed lighting, unpolished. Not studio."
```

Same product, different prompt, meaningfully different result. See `tiktok-video-generation`.

## The feed crop problem

The constraint that catches people out. A 9:16 Reel appears in the main feed and on the profile grid **cropped**, and if you composed for full-screen the crop cuts your subject.

```
9:16 full screen            Feed preview (~4:5)        Grid (1:1)
┌───────────┐               ┌───────────┐              ┌───────────┐
│           │  ← cropped    │           │              │           │
│           │               │  visible  │              │  visible  │
│  subject  │               │  subject  │              │  subject  │
│           │               │           │              └───────────┘
│           │  ← cropped    └───────────┘
└───────────┘
```

**Compose the subject in the vertical centre**, so it survives a 4:5 and a 1:1 crop. That's a tighter constraint than TikTok's, where only the platform UI matters.

Combined with the UI safe area, the usable zone on Reels is narrower than on TikTok:

- **Keep the subject in the middle 50-55%** vertically, not 60%
- **Top ~12%** carries the account name and audio
- **Bottom ~25%** carries caption, buttons and CTA
- **Right ~12%** carries the action buttons

See `aspect-ratio-strategy`.

## The generation call

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[PRODUCT_ASSET],
  prompt="Product centred in the vertical middle of frame, generous "
         "space above and below. Hands enter and lift it. Soft "
         "directional daylight, clean composition, shallow depth of "
         "field, warm grade. Subtle handheld movement. Camera locked.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=6,
  seed=4271
)
```

Note **"generous space above and below"** — that's the feed-crop insurance, and it has to be prompted at generation time.

For paid Reels, render at 1080p / `fhd`. For organic, 720p / `hd` is fine — Instagram recompresses hard. See `video-model-selection`.

## Length and pacing

Reels tolerates slightly longer than TikTok, and rewards it less than people assume.

- **6-15 seconds** is the productive range for most content
- **Under 10s** for anything ad-like
- **Loops work unusually well** on Reels — a seamless 5-second loop accumulates watch time as it replays, which reads as high retention

The loop point is worth actual effort here. Prompt for motion that returns to its start position, then fix the join in the edit. See `video-duration-pacing`.

## Sound and captions

Assume muted for the first viewing, same as everywhere.

- **Captions in the middle third.** Not the bottom, where the platform's caption sits
- **Trending audio matters less than on TikTok** but still contributes to distribution
- **Original audio is more accepted** on Reels than on TikTok
- **Don't render text into the clip** — you'll want variants and localisation. See `video-caption-subtitle-planning`

## Reels as an ad

When it's paid rather than organic, several things change:

- **1080p.** Ad review and larger placements are less forgiving
- **Front-load the brand more than organic**, though still not in frame one
- **Claim substantiation applies fully.** Generated visuals get the same standard as filmed. See `video-brand-safety-moderation`
- **AI disclosure.** Meta requires disclosure of realistic AI-generated content in ads and applies its own labelling, which can affect delivery if you didn't declare it
- **Aspect ratio for placements.** A Reels ad may also serve in feed and Stories — generate 9:16 and 1:1 natively rather than letting the platform crop

## The production loop

```
1. generate_image        hero still, 9:16, subject vertically centred
2. create_brand_asset    save product and presenter — free
3. generate_video (draft) 4-6s beats, locked seed, several hook variants
4. judge muted, on a phone, and check the 1:1 crop
5. enhance_video_draft    winners only
6. assemble + caption     in an editor
```

Step 4's crop check is the Reels-specific step. Drop your frame into a 1:1 and a 4:5 box and confirm the subject survives.

## Don't

- **Don't post the TikTok file unchanged.** Different aesthetic, different crop constraint.
- **Don't compose for full-screen only.** Reels crops in feed and grid.
- **Don't prompt for deliberate roughness** here by reflex. Reels tolerates polish.
- **Don't put captions at the bottom.**
- **Don't render text into the clip.**
- **Don't skip the AI disclosure** on paid Reels.
- **Don't ignore the loop opportunity.** It's more valuable here than on TikTok.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`tiktok-video-generation`, `aspect-ratio-strategy`, `video-caption-subtitle-planning`, `video-hook-first-3-seconds`, `video-duration-pacing`, `video-brand-safety-moderation`, `ugc-vs-polished-decision`
