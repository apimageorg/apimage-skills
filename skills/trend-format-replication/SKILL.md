---
name: trend-format-replication
description: Adapt a trending short-form format to your product without copying the content or infringing rights. Use whenever the user wants to use a trend, replicate a viral format, mentions trending audio or a viral template, or asks how to make generated video feel current.
---

# Replicating Trend Formats

Trends are a distribution advantage: the platform is already surfacing the format, and the audience already understands it, so your content gets comprehension and reach for free.

The trap is copying the wrong layer. A trend has a **structure** and it has **content**. The structure is fair game and reusable. The content usually belongs to someone.

## Separate the structure from the content

```
The trend:        A specific creator's clip, specific audio,
                  specific footage, specific joke
The structure:    Problem shown → attempt fails → reveal → reaction
The audio:        Someone's track, licensed or not
The visual style: Handheld, jump cut on the beat, text at 2s
```

**Replicate the structure. Never lift the content.** Re-creating someone's exact clip with your product substituted is derivative work, and on a platform where the original is still circulating it also looks like what it is.

The structure — the beat pattern, the reveal timing, the text placement rhythm — is a format. Formats get reused constantly and legitimately.

## Deconstruct before you generate

Watch the trend three or four times and write down the mechanics, not the content.

```
Trend: [whatever it is this month]

Beat 1   0-1.5s   Static frame, text appears, no motion
Beat 2   1.5-3s   Hard cut on the beat, subject enters
Beat 3   3-5s     Fast cuts, 3 shots, escalating
Beat 4   5-7s     Hold. The reveal. No cut
Beat 5   7-8s     Reaction / result

Text:    appears at 0.5s, upper-middle, 4 words
Pace:    cuts land on beats 1, 2 and 4 of the bar
Look:    handheld, over-exposed, phone-camera
```

That's a reusable template. Nothing in it is anyone's property.

## Generate against the template

```python
LOOK = ("Handheld phone footage, slight shake, natural indoor light, "
        "slightly over-exposed, unpolished. Not studio, not commercial.")

BEATS = [
    ("static",  "Static frame, product on the counter, nothing moves.", 2),
    ("enter",   "Hands enter frame fast and grab the product.", 2),
    ("escalate","Quick close-up, product being applied, fast motion.", 2),
    ("reveal",  "Hold on the result, no camera movement.", 3),
    ("react",   "Hands lift the finished result toward camera.", 2),
]

specs = [{
    "mode": "image-to-video",
    "model": "flux-3-video-draft",
    "reference_images": [PRODUCT_ASSET],
    "prompt": f"{action} {LOOK} Everything else stays still.",
    "aspect_ratio": "9:16",
    "resolution": "hd",
    "duration": dur,
    "seed": 4271,
} for _, action, dur in BEATS]
```

Generate the beats, cut them to the template's rhythm in an editor. The pacing is where the trend lives, and pacing is an edit decision, not a generation parameter. See `multi-scene-video-assembly`.

## Audio: the part that actually has rules

Trending audio is a real distribution factor and a real rights problem.

- **Use the platform's own audio library.** TikTok, Reels and Shorts each provide licensed trending sounds for use *on that platform*. Using them there is fine
- **Don't export platform audio and reuse it elsewhere.** The licence covers use within the app, not as a downloaded asset in a paid ad or on your website
- **Paid ads have different rules.** Trending sounds are frequently not cleared for advertising use. Commercial music libraries exist for this
- **Don't pass music into `generate_lip_sync`.** It interferes with the sync. Add music in the edit
- **Don't generate the audio.** APImage generates video and images, not soundtracks

The common expensive mistake: building an ad around a trending sound, then discovering it isn't cleared for paid use. Check before you build. See `music-audio-pairing`.

## Timing: trends decay fast

| Age | Value |
|---|---|
| 0-3 days | Peak, and hard to get into fast enough |
| 4-10 days | **The productive window** |
| 10-20 days | Declining, still workable |
| 20+ days | Late. Reads as behind |
| 6 weeks+ | Actively bad. Looks out of touch |

**The generation speed is the advantage here.** Producing a trend response used to mean a shoot; generating five beats and cutting them is an afternoon. That's what makes the 4-10 day window reachable.

Keep a saved asset set — product, background, preset — so a trend response starts from a fixed visual world rather than from scratch. See `character-consistency-video` and `batch-video-production`.

## When not to chase a trend

- **The structure doesn't fit the product.** Forcing it reads as desperate and it's the most common failure
- **The trend is a joke that requires being in on it.** Brand accounts do this badly
- **The trend involves a person, a voice or a character** that isn't yours
- **The trend is about something serious.** Attaching a product to it is the fastest way to a public mistake
- **Your audience isn't on the platform where it's trending**
- **It's already peaked**

**The fit test:** does the structure carry your message, or are you bending your message to fit the structure? If the second, skip it. A trend-shaped clip that says nothing performs worse than an original clip that says something.

## Evergreen formats beat trends

Worth saying, because it's where the durable value is. Several short-form structures work permanently rather than for two weeks:

- Problem → attempt → solution
- Before → after
- Three things about X
- What I expected vs what happened
- Result first, then how
- A common mistake, corrected

These need no timing, no audio licence and no trend monitoring, and they perform consistently. Build the library on these and use trends opportunistically on top. See `video-hook-first-3-seconds`.

## Don't

- **Don't recreate someone's exact clip** with your product substituted.
- **Don't export platform audio** for use outside the platform.
- **Don't use trending sounds in paid ads** without checking clearance.
- **Don't force a structure that doesn't fit** the product.
- **Don't attach a product to a serious trend.**
- **Don't chase a trend past three weeks.**
- **Don't generate the audio.** APImage does video and images.
- **Don't build the whole strategy on trends.** Evergreen formats compound.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`tiktok-video-generation`, `multi-scene-video-assembly`, `video-hook-first-3-seconds`, `batch-video-production`, `video-brand-safety-moderation`, `ugc-duet-response-format`
