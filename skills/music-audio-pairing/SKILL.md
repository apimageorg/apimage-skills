---
name: music-audio-pairing
description: Add music, sound design and voiceover to generated video, including the licensing rules that catch people out. Use whenever the user is adding audio to video, mentions music or sound design or trending audio or voiceover, or asks whether they can use a track in an ad.
---

# Music and Audio

**APImage generates video and images, not audio.** Every soundtrack, sound effect and voiceover is added in an editor afterwards. That's worth stating because people look for an audio parameter and there isn't one.

The one exception is `generate_lip_sync`, which *consumes* audio you supply rather than producing it.

## The licensing rules, first

This is where money and legal exposure actually sit, and it's the part most often got wrong.

| Source | Organic on that platform | Paid ads | Website / email |
|---|---|---|---|
| Platform audio library (TikTok, Reels, Shorts) | **Yes** | **Usually no** | **No** |
| Commercial music library (licensed) | Yes | Yes, per licence | Yes, per licence |
| Royalty-free with commercial licence | Yes | Check the licence | Check the licence |
| Popular commercial track | No | No | No |
| Audio ripped from another creator's video | No | No | No |

**The trap: platform audio libraries are licensed for use inside that platform, not as a downloaded asset.** Building a paid ad around a trending TikTok sound, then discovering it isn't cleared for advertising, is a common and expensive discovery — usually made after the creative is finished.

For anything paid, or anything that leaves the platform, licence the music properly. It costs less than reshooting.

See `trend-format-replication` for the trend-audio specifics.

## Music choice by format

| Content | Music |
|---|---|
| UGC-style ad | Minimal or none. Room sound reads as authentic |
| Product demo | Subtle bed, low. Let the product sound lead |
| Transformation | Builds to the reveal. The payoff needs a lift |
| Faceless explainer | Low, neutral, unobtrusive. It's carrying narration |
| Brand film | Considered, licensed, mixed properly |
| Trend response | Whatever the trend uses, on-platform only |

**Less is usually better than more.** Loud music over a generated clip draws attention to production values, which is the opposite of what UGC-style content wants.

## Sound design earns more than music

The genuinely under-used lever. Generated video has no diegetic sound at all — no pour, no click, no wipe, no footstep — and adding it makes the visual feel real in a way music doesn't.

| Visual | The sound that sells it |
|---|---|
| Liquid pouring | The pour, close and detailed |
| A lid or clasp opening | The click |
| Wiping a surface | The friction |
| Fabric moving | The rustle |
| Something being set down | The contact |
| Steam or spray | The hiss |

**One well-placed sound effect does more for perceived realism than any amount of music.** It's also cheap and quick.

For transformation content this is close to essential — the wipe, the click, the pour is where the satisfaction lives. See `before-after-transformation-video`.

## Cut to the audio, not the other way round

The pacing decision that makes an edit feel intentional.

- **Place cuts on the beat.** A cut landing off-beat feels accidental even when the shot is good
- **Let the music tell you the duration.** An 8-bar phrase is a natural clip length
- **Build to the reveal.** Rising music into the payoff beat, then hold
- **Drop the music for the CTA** if you want it heard. Silence draws attention

Generate beats with trim margin so you have frames to cut on. A clip generated at exactly the needed length forces the cut onto its weakest frames. See `video-duration-pacing` and `multi-scene-video-assembly`.

## Assume it's watched muted

Most feed viewing starts silent. Everything above matters for the sound-on viewing; the sound-off viewing has to work independently.

- **The visual carries the message alone**
- **Every spoken claim has an on-screen equivalent.** See `video-caption-subtitle-planning`
- **Music is never load-bearing.** If the clip only works with the track, it doesn't work
- **Judge the clip muted first**, then with sound

The practical order: make it work muted, then add audio to make it better. Not the reverse.

## Voiceover

Recorded or synthesised, both normal.

**For `generate_lip_sync`, the audio requirements are strict** because it drives the mouth:

- Clean, no background noise, no clipping
- **No music in the track.** It interferes with the sync. Add music afterwards, in the edit
- Measured pace — fast delivery syncs worse
- Clear plosives (P, B, M), which produce visible mouth shapes

```
generate_lip_sync(
  face_image="<character asset>",
  audio="<clean speech, no music, under 30s>"
)
```

Music added later, over the finished clip. See `lip-sync-spokesperson-video`.

**For a synthetic voice presented as a person:** don't attribute it to a named real individual, and disclose AI content where the platform requires it. See `video-brand-safety-moderation`.

## Mixing

Rough targets, and the common error is music too loud:

```
Voiceover        the loudest element, clearly intelligible
Sound design     present, supporting — not competing with speech
Music            well under the voice. Duck it under speech
Overall          normalise to the platform's loudness target
```

**Platforms normalise loudness.** Mastering hot doesn't make you louder in the feed — it just makes you more compressed than everyone else. Mix for clarity.

## Don't

- **Don't look for an audio parameter.** APImage generates video and images.
- **Don't use platform library audio outside that platform.**
- **Don't use trending sounds in paid ads** without checking clearance.
- **Don't use a commercial track you haven't licensed.**
- **Don't pass music into `generate_lip_sync`.**
- **Don't make music load-bearing.** It has to work muted.
- **Don't skip sound design.** It does more than music for realism.
- **Don't master hot.** Platforms normalise.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lip-sync-spokesperson-video`, `video-caption-subtitle-planning`, `multi-scene-video-assembly`, `trend-format-replication`, `video-duration-pacing`, `before-after-transformation-video`, `ugc-asmr-sensory-format`
