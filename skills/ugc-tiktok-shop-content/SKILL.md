---
name: ugc-tiktok-shop-content
description: Produce shoppable short-form for TikTok Shop and in-app checkout, where the video has to close the sale in the app. Use whenever the user is producing content for TikTok Shop, Instagram or YouTube shopping, live-commerce clips, or video with an in-app buy button.
---

# TikTok Shop and Shoppable Video

Shoppable video is a different job from an ad. There's no landing page to finish the argument — the product card is right there, and the video has about fifteen seconds to answer every question standing between a scroll and a tap.

Which means the content is less persuasion and more **answering objections fast**.

## What changes when the buy button is in frame

| Ad creative | Shoppable creative |
|---|---|
| Build interest, drive a click | Answer objections, drive a tap |
| Landing page does the detail | The video is the detail |
| Brand consistency matters | Almost irrelevant |
| Polish acceptable | Rough outperforms clearly |
| One message | Several answers, fast |
| Hook is everything | Hook plus the practical answers |

**The purchase is impulsive and fully informed at once**, which is unusual. The viewer will buy in ten seconds if — and only if — they know the size, the price, what it does and what it doesn't.

## The structure

```
0-3s     HOOK           the problem, or the result. Specific
3-6s     WHAT IT IS     plainly. No brand language
6-9s     THE PROOF      it working. Demonstrated, in one shot
9-12s    THE OBJECTION  size, price, compatibility, or shipping
12-15s   THE ACTION     "it's in the link below". Explicit
```

**The objection beat is what separates shoppable from ad creative**, and it's the one most often left out. The single most common reason a tap doesn't happen is an unanswered practical question.

Objections worth spending three seconds on:

```
Size and quantity      "It's 250ml, lasts about a year"
Price framing          "Forty quid, and you use a teaspoon at a time"
Compatibility          "Oil-finished wood only. Not lacquer"
Shipping and returns    "Two days, and returns are free"
How many you need      "One bottle does a whole kitchen"
```

## Show the size. On screen. Every time.

Size confusion is the biggest driver of shoppable returns, and returns on marketplace commerce hurt more than a lost sale — they hit the seller metrics that govern your reach.

- **A hand in frame** in at least one shot
- **Dimensions or volume as on-screen text**, not spoken only
- **Beside a known object** — a mug, a phone, a plug socket
- **Never generate a flattering scale.** Check against the real dimensions

See `product-scale-reference`.

## Producing it

Fast, cheap, no presenter needed for most of it.

```python
PROD = ["<front>", "<in hand>", "<back label>"]
LOOK = ("Handheld phone footage, slight shake, slightly uneven "
        "exposure. Natural window light from the left, no fill. "
        "Ordinary kitchen worktop with a mug and some clutter. "
        "Unpolished, shot on a phone. Not studio, not commercial.")
SEED = 4271

BEATS = [
  ("Very close on a pale ring mark on the worktop, a fingertip "
   "traces it. No product in frame.", 3),
  ("A hand holds the bottle beside a mug on the worktop, turning "
   "it so the front label is clearly readable. Held steady.", 3),
  ("A hand presses a cloth onto the mark and buffs in small "
   "circles, the mark fading. Camera locked on the same spot.", 5),
  ("Close on the same patch, now unmarked, then the bottle set "
   "down beside it.", 4),
]
```

**The second beat is the shoppable-specific one:** product beside a scale reference with a legible label, held steady long enough to read. Every shoppable video needs it.

Draft the set, enhance what works. Video is async — fire the beats, then poll `get_video_generation`. Polling is free. See `async-video-job-orchestration` and `draft-then-enhance-workflow`.

## Marketplace accuracy rules apply

Shoppable content sits under commerce policy, not just advertising policy. The consequences are operational: listing suppression, commission holds, account restriction.

```
[ ] Every claim matches the product listing exactly
[ ] Price shown matches the current listing price
[ ] Nothing shown that isn't in the box
[ ] Real product photography as the reference throughout
[ ] No generated packaging or printed text
[ ] Current packaging only
[ ] No health, medical or safety claims unless permitted and evidenced
[ ] Restricted-category rules checked before publishing
```

**Video and listing must agree.** A shoppable video is effectively part of the listing, and a mismatch between the two is the most common enforcement trigger. See `marketplace-image-compliance`.

## Disclosure, twice over

Shoppable content usually needs two labels, and they're separate obligations.

- **Commercial disclosure.** A brand account with a product card is advertising. Platforms also require the paid-partnership or branded-content toggle for creator content
- **AI-content disclosure.** Generated footage needs the platform's synthetic-media label
- **Affiliate relationships** disclosed in-video, not caption-only. Heavily enforced in commerce
- **Don't let a generated presenter claim to have bought or used it.** Commerce content is where fabricated endorsement is most consequential, because it sits directly on a transaction

See `ugc-disclosure-compliance` and `ugc-affiliate-creative`.

## Volume is the strategy

Shoppable creative fatigues faster than ad creative — the audience is narrower and the same product card recurs.

```
Per SKU:   8-12 variants
Vary:      hook, objection answered, creator, setting
Fix:       product references, look clause
Refresh:   weekly. Hooks first, bodies later
```

The economics work because the beats are short and drafting is cheap. One product asset set plus a fixed look clause makes each new variant a prompt change. See `ugc-batch-testing` and `batch-video-production`.

**Track per-variant conversion, not views.** Shoppable analytics give you the tap and the purchase, which is a far better signal than watch time — and it means you can kill variants in a day rather than a week. See `ugc-performance-iteration`.

## Don't

- **Don't skip the objection beat.**
- **Don't leave size unstated.**
- **Don't let the video and the listing disagree.**
- **Don't show a price you haven't just checked.**
- **Don't polish it.** Rough converts better here.
- **Don't generate packaging or label text.**
- **Don't let a synthetic presenter claim a purchase.**
- **Don't run one creative per SKU.** Volume and refresh.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`tiktok-video-generation`, `social-commerce-product-images`, `ugc-affiliate-creative`, `product-scale-reference`, `marketplace-image-compliance`, `ugc-batch-testing`
