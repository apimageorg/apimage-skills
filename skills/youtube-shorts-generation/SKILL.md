---
name: youtube-shorts-generation
description: Generate video for YouTube Shorts specifically — search intent, longer tolerance, channel context and how it differs from TikTok and Reels. Use whenever the user wants Shorts content, YouTube vertical video, faceless YouTube automation, or asks how Shorts differs from other short-form platforms.
---

# YouTube Shorts Generation

Shorts sits on YouTube, and YouTube is a search engine. That single fact makes it behave differently from TikTok and Reels in ways that change what you should generate.

## What's different

| | TikTok / Reels | Shorts |
|---|---|---|
| Discovery | Almost entirely algorithmic feed | Feed **plus search plus suggested** |
| Content lifespan | Days | **Months to years** — search keeps working |
| Title and description | Cosmetic | **Ranking signals.** Real SEO weight |
| Channel context | Weak | Strong. Subscribers and watch history matter |
| Length tolerance | 6-15s | Up to 60s, and longer often performs |
| Crossover value | None | **Shorts drive long-form subscriptions** |
| Aesthetic | Native/rough rewarded | Informational clarity rewarded |

**The lifespan difference is the strategic one.** A TikTok is a spike; a Short that answers a searched question keeps accumulating views for a year. That makes evergreen, question-answering content substantially more valuable here than trend-chasing.

## Generate for search intent

Because Shorts surfaces in search and suggested, the content should answer something people look for.

```
Weak (trend-shaped):    "satisfying product clip with trending audio"
Strong (search-shaped): "how to remove a coffee stain from oak"
```

The second one gets found for a year. The first one gets a day.

Practically: pick the question first, generate the visual that answers it, and title it as the question. The title is a ranking input on YouTube in a way it simply isn't on TikTok.

## Length: use the extra room

Shorts tolerates up to 60 seconds and often rewards using it, which is the opposite of the TikTok advice.

| Duration | Use for |
|---|---|
| 10-20s | Single-point answer, satisfying clip |
| 20-35s | **The productive range.** A real explanation with beats |
| 35-60s | A full how-to or a multi-step demo |

Even so: **generate beats and assemble.** A 35-second Short is six or seven generated clips cut together, not one 35-second generation. Long single generations drift. See `multi-scene-video-assembly` and `video-duration-pacing`.

## Safe area

Shorts covers less of the frame than TikTok, but the constraint still exists.

- **Bottom ~15-20%** carries the title, channel name and buttons
- **Right ~10%** carries like, dislike, comment, share
- **Top** is relatively clear compared to other platforms

Slightly more usable frame than TikTok, and still: keep the subject centred and captions in the middle third. See `aspect-ratio-strategy`.

## The generation call

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[STILL],
  prompt="Close-up, hands applying the solution to the stained oak. "
         "Clear, well-lit, informational. Camera locked, subject "
         "centred with space at the bottom of frame. Natural daylight, "
         "neutral grade. Steady, unhurried.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

**"Clear, well-lit, informational"** is the register that works here. Shorts audiences are frequently in learning mode rather than entertainment mode, and legibility beats atmosphere. Prompting for moody, shallow, cinematic looks tends to hurt comprehension.

## Faceless channels

Shorts is the primary platform for faceless automation, and generated video suits it well.

The pattern:

```
1. Pick a searched question
2. generate_image        the key visuals — 4-6 stills
3. generate_video (draft) animate each as a 4-5s beat
4. enhance the keepers
5. Assemble + voiceover + captions in an editor
6. Title as the question. Description with detail
```

Consistency across a faceless channel is what makes it read as a channel rather than a scrape. Save a background asset and a preset, use the same look clause verbatim, and hold a seed family per series. See `character-consistency-video` and `batch-video-production`.

**On the platform rules:** YouTube's policies target *inauthentic, mass-produced, repetitive* content with no added value. Generated visuals are fine. A channel publishing forty near-identical clips with synthetic narration and no original insight is the thing the policy exists for. The distinction is whether each video genuinely answers something.

YouTube also requires disclosure of realistic synthetic content and applies its own labelling. Declare it. See `video-brand-safety-moderation`.

## Titles, descriptions and thumbnails

The parts that don't exist on TikTok and carry real weight here.

- **Title as the search query.** "How to remove coffee stains from oak" not "This actually works 🤯"
- **Description with substance.** It's indexed. A few sentences of real detail plus relevant terms
- **Thumbnails matter for Shorts** in suggested and on the channel page, even though the feed autoplays. See `video-thumbnail-generation`
- **Consistent series naming** helps YouTube understand the channel

This is the one short-form platform where an hour spent on metadata changes the outcome.

## Shorts feeding long-form

If there's a long-form channel, Shorts is the top of that funnel. Two things follow:

- **Keep the visual language consistent** between Shorts and long-form, so a viewer who subscribes recognises the channel
- **End on something that implies more** — a related question, a reference to a fuller version

Generated Shorts as an acquisition layer for a real channel is a stronger position than generated Shorts as the whole product.

## Don't

- **Don't post the TikTok file with a TikTok title.** Titles rank here.
- **Don't chase trends.** Search intent has a longer payoff on this platform.
- **Don't generate one 35-second clip.** Beats, assembled.
- **Don't use a moody cinematic look** for informational content. Clarity wins.
- **Don't publish forty near-identical clips.** That's what the inauthentic-content policy targets.
- **Don't skip the description.** It's indexed.
- **Don't skip the AI disclosure.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`tiktok-video-generation`, `instagram-reels-generation`, `multi-scene-video-assembly`, `aspect-ratio-strategy`, `batch-video-production`, `video-duration-pacing`, `ugc-tutorial-demo`
