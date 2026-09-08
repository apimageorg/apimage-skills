---
name: ugc-affiliate-creative
description: Produce affiliate and partner creative, and supply generated assets to affiliates without inheriting their compliance failures. Use whenever the user runs an affiliate programme, supplies creative to partners, produces commission-based content, or needs a creative kit for creators.
---

# Affiliate and Partner Creative

Affiliate creative has a structural problem: the person publishing is not the person legally on the hook, and the brand usually is anyway. Regulators and platforms increasingly treat the advertiser as responsible for what its affiliates publish.

Which makes **supplying compliant creative** the highest-leverage thing a programme can do. Generation makes that cheap, because you can produce a large, varied kit at low cost rather than hoping affiliates make their own.

## Two different jobs

| Job | Who publishes | What's needed |
|---|---|---|
| **Affiliate publishing your creative** | The affiliate | Ready-made, pre-disclosed assets |
| **You publishing affiliate content** | You | Licence from the creator |
| **Affiliate making their own** | The affiliate | Rules and a claims list |

The first is where generation pays off, and it's also where compliance is controllable. Give affiliates good assets and they use them; give them nothing and they invent claims.

## Build a creative kit

The deliverable that solves the problem. Produce it once per SKU.

```
Video
  6-10 hook variants          4s each, drafted
  2-3 body clips              enhanced, full quality
  2-3 result beats
  1 clean product beat        legible label, scale reference

Stills
  1 clean anchor image
  4-6 UGC-style stills
  1 scale-reference shot

Text
  Approved claims list        with substantiation on file
  Prohibited claims list      explicit
  Required disclosure wording
  Current price and its basis, with a review date
```

**The prohibited-claims list is the part that actually protects you.** Affiliates over-claim because nobody told them where the line is, and "use your judgement" isn't guidance.

Fix the product references and look clause across the kit so everything an affiliate publishes is visually consistent with your own creative. Brand assets are free — save the product, background and preset once. See `ugc-character-consistency` and `batch-video-production`.

## Pre-disclose the assets

The single most effective control: burn the disclosure into the asset so the affiliate can't omit it.

- **On-screen text in the first three seconds** of every video asset
- **Plain words.** "Ad" or "Paid partnership", not a hashtag
- **AI-content label** where the footage is generated. Separate obligation
- **Don't rely on the affiliate adding it.** Many won't, and it's your exposure

**A caption-only disclosure fails** on most platforms and with most regulators: captions are truncated, collapsed and skipped. In-video is the only version that reliably counts. See `ugc-disclosure-compliance`.

## What affiliates get wrong, and how the kit prevents it

| Failure | Prevention |
|---|---|
| No disclosure | Burned into the asset |
| Invented claims | Approved-claims list, prohibited list |
| Stale prices | Price sheet with a review date, and reissue |
| Health or medical claims | Explicitly prohibited, in writing |
| Fake urgency | No countdowns or stock claims in the kit |
| Competitor disparagement | Prohibited, in writing |
| Fabricated personal experience | Assets that make no experience claim |
| Wrong or old packaging | Only current assets supplied, old ones withdrawn |

**Withdraw superseded assets actively.** Affiliates keep using last year's creative indefinitely unless you tell them the packaging changed and re-issue.

## The assets should make no experience claim

Since you don't control who publishes them, build the kit so the assets are safe in anyone's hands.

```
Safe in any hands:   product demonstrated, hands only, no dialogue
                     "here's what this does"
                     mechanism explained
                     a stated limitation

Not safe:            "I've used this for months"
                     a synthetic presenter implying purchase
                     any first-person result claim
```

**Hands-only and product-only assets are the ideal affiliate kit** — no identity, no testimony, no consistency problem, and an affiliate can voice them over in their own genuine words. That's the combination that's both compliant and effective: your visuals, their real experience. See `b-roll-generation` and `faceless-video-automation`.

## Using an affiliate's content yourself

The reverse direction, and it needs paperwork.

- **A written usage licence.** Scope, channels, duration, paid or organic, territory
- **Their platform post is not a licence** for you to run it as an ad
- **Whitelisting or partnership ads** need the platform's own permission flow, granted by them
- **Their claims become your claims** once you amplify. Substantiate before boosting
- **Don't extend generated scenery around their footage** in a way that changes what they appeared to say or do
- **Music rights don't transfer.** Their trending audio is not licensed for your ad

See `ugc-legal-likeness-rights` and `ugc-duet-response-format`.

## Programme mechanics that reduce risk

- **A single asset library** with versions and withdrawal dates
- **Terms that require in-video disclosure**, with removal for breach
- **Spot-check published content** monthly. Programmes drift
- **A claims sheet per SKU**, reissued when the product or price changes
- **No self-attribution.** Affiliates must not present supplied generated assets as their own footage of their own purchase
- **Log what you issued, to whom, when.** When a complaint arrives, that record is the difference between a fixable problem and an unbounded one

## Don't

- **Don't let affiliates write their own claims.**
- **Don't rely on affiliates to add disclosure.** Burn it in.
- **Don't use caption-only disclosure.**
- **Don't leave superseded assets in circulation.**
- **Don't supply assets that claim personal experience.**
- **Don't boost an affiliate's post** without a licence.
- **Don't assume their music rights** cover your ad.
- **Don't skip the spot-checks.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-disclosure-compliance`, `ugc-legal-likeness-rights`, `ugc-tiktok-shop-content`, `batch-video-production`, `ugc-multi-creator-variants`, `ugc-haul-format`
