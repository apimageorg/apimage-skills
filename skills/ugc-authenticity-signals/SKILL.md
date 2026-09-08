---
name: ugc-authenticity-signals
description: The specific visual and audio cues that make generated content read as real rather than produced, and how to prompt for them. Use whenever generated content looks too polished, reads as an ad, or the user asks how to make AI video or images look authentic and native.
---

# Authenticity Signals

Generative models are trained toward the aesthetic of good commercial photography, so their default output is clean, well-lit, symmetrical and evenly exposed. That aesthetic is exactly what a feed reads as "advert" and scrolls past.

Making generated content look native means prompting *against* the model's competence. This skill is the list of what to prompt for.

## The signal inventory

| Reads as produced | Reads as real |
|---|---|
| Even studio lighting | Natural window light, one source, visible falloff |
| Smooth gimbal camera move | Handheld, slight shake |
| Perfect exposure | Slightly over or under, uneven |
| Pristine background | Ordinary clutter — a mug, a cable, a folded cloth |
| Subject centred | Slightly off-centre, casual framing |
| Colour graded | Neutral or slightly imperfect white balance |
| Sharp throughout | Focus that's a little soft, or slightly missed |
| Product hero-lit on a plinth | Product held, in use, partly out of frame |
| Logo in frame | No branding until the end |
| Symmetrical composition | Asymmetric, unconsidered |
| Silent or scored | Room sound, background noise |
| Studio voiceover | Direct-to-camera, room acoustics |

**Clutter is the highest-leverage single addition.** A pristine kitchen counter reads as a set in about half a second. One mug, one cloth, one visible crumb reads as a kitchen.

## The prompt clauses

Copy these. They're the working vocabulary.

```
Camera:      "handheld phone footage, slight shake, casual framing"
Lighting:    "natural window light, single source, no fill light,
              visible falloff"
Exposure:    "slightly uneven exposure, not perfectly lit"
Setting:     "ordinary [room] with some everyday clutter visible —
              a mug, a folded cloth, a cable"
Framing:     "subject slightly off-centre, unconsidered framing"
Focus:       "shallow depth of field, focus slightly soft"
Grade:       "neutral, ungraded, natural colour"
Negative:    "unpolished, shot on a phone. Not studio, not
              commercial, not advertising photography"
```

**The negative framing at the end does real work.** "Not studio, not commercial, not advertising photography" pushes against the training prior more effectively than any amount of positive description.

## A worked comparison

**Default output — reads as an ad:**
```
A woman holding a skincare bottle in a bright modern kitchen,
professional lighting, high quality, 4k, beautiful
```

Every word there pushes toward polish. "Professional lighting", "high quality", "4k" and "beautiful" are all instructions to produce an advert.

**Native output:**
```
She holds the bottle up toward the camera, already mid-sentence,
natural small head movements. Handheld phone footage, slight shake,
slightly uneven exposure, subject a little off-centre. Ordinary
kitchen behind with a mug and a folded tea towel visible on the
counter. Natural window light from the left, no fill. Unpolished,
shot on a phone. Not studio, not commercial.
```

Longer, more specific, and every clause is doing something.

## Signals that go too far

Authenticity has a floor. Deliberate roughness past a point reads as low effort rather than as real, and it costs credibility.

| Acceptable | Too far |
|---|---|
| Slight camera shake | Unwatchable motion |
| Slightly uneven exposure | Blown out or unreadably dark |
| Some background clutter | Genuinely messy or unhygienic |
| Casual framing | Subject half out of frame |
| Focus slightly soft | Out of focus |
| Room sound | Unintelligible audio |
| No colour grade | Colour cast that misrepresents the product |

**That last row is the one with commercial consequence.** A green colour cast in the name of authenticity that makes the product look the wrong colour is a return, not a style. Product accuracy holds regardless of the register. See `product-image-qa-review`.

## Audio signals

Often more powerful than visual ones, and they cost nothing because they're added in the edit.

- **Room sound.** Silence reads as studio. A faint room tone reads as a real place
- **Direct-to-camera delivery** with natural pace and the occasional stumble
- **Diegetic sound** — the pump, the click, the pour, the fabric. See `music-audio-pairing`
- **No music, or very low.** Loud music over a UGC ad reads as produced immediately
- **Slight background noise** — distant traffic, a kettle, a door

**No music is a legitimate and underused choice** for UGC. It's what a real creator's clip sounds like.

## Which platforms want it

Authenticity is not universally rewarded. Choose deliberately.

| Platform | Register |
|---|---|
| TikTok | **Rough. Strongly rewarded** |
| TikTok Shop | **Rough** |
| Instagram Reels | Considered — polish is tolerated and often rewarded |
| Instagram feed | Polished |
| Pinterest | **Polished, aspirational** |
| YouTube Shorts | Clear and informational over either |
| Facebook feed | Native performs |
| Paid social | Test both. Often native wins |
| Brand website | Polished |

Defaulting to rough everywhere is as much a mistake as defaulting to polished. See `ugc-vs-polished-decision`.

## The honest limit

Authenticity signalling makes content *look* like it was made by a person. It doesn't make it *have been* made by a person, and that distinction matters for what you're allowed to claim.

Prompting for handheld shake and kitchen clutter is a legitimate visual choice. Using that visual register to present a synthetic person as a real customer sharing genuine experience is a false endorsement — and it's the specific thing platforms and regulators are targeting.

**Use the aesthetic. Don't use it to fabricate testimony.** See `ugc-disclosure-compliance`.

## Don't

- **Don't write "professional", "high quality" or "4k"** in a UGC prompt. They push the wrong way.
- **Don't omit the negative framing.** "Not studio, not commercial" earns its place.
- **Don't skip the clutter.** Highest-leverage single signal.
- **Don't use fill light.** Single-source with falloff.
- **Don't let a colour cast misrepresent the product.**
- **Don't go past watchable** in pursuit of rough.
- **Don't add loud music** to a UGC ad.
- **Don't use the aesthetic to fake testimony.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-ad-video-generation`, `ugc-vs-polished-decision`, `tiktok-video-generation`, `music-audio-pairing`, `ugc-disclosure-compliance`, `ugc-selfie-style-photos`
