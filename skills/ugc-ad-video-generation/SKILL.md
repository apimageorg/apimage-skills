---
name: ugc-ad-video-generation
description: Generate UGC-style ad video from a product photo — the core APImage workflow — with the structure, look and disclosure that make it work. Use whenever the user wants UGC ads, creator-style video, authentic-looking product ads, or asks how to turn a product photo into an ad video.
---

# UGC-Style Ad Video

The core APImage use case: a product photo in, an ad that looks like a creator made it out. It works because UGC-style creative reliably outperforms polished brand video on social — the feed treats it as content rather than as an advert.

Two things decide whether it lands: whether it looks native, and whether you're honest about what it is.

## The structure

UGC ads follow a consistent shape. It's not a formula to hide, it's the shape the format has because it works.

```
0-3s    HOOK        a person, mid-sentence or mid-action. A problem or claim
3-6s    PROBLEM     what was wrong, specifically
6-12s   PRODUCT     the product, in use, in a real place
12-16s  RESULT      what changed
16-18s  CTA         one action, casual
```

**Generate each beat as a separate clip and cut them.** A single 18-second generation drifts and morphs; five short beats hold identity and let you fix one without re-rendering all of it. See `multi-scene-video-assembly`.

## The production loop

```
1. generate_image           the creator portrait — iterate at 1-9 credits
2. create_brand_asset       save as a character. Free
3. Product photo            the real one, cut out and saved as an asset
4. generate_video (draft)   each beat, locked seed, cheap iterations
5. enhance_video_draft      the keepers only
6. Assemble                 cuts, captions, sound in an editor
```

Everything expensive happens once, on something already approved. See `draft-then-enhance-workflow` and `video-credit-cost-management`.

## Prompt against the polish

This is the whole craft. The models default to commercial gloss, and commercial gloss is what the feed skips.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[CREATOR_ASSET, PRODUCT_ASSET],
  prompt="She holds the bottle up toward the camera, already "
         "mid-sentence, natural small head movements. Handheld phone "
         "footage, slight shake, slightly uneven exposure. Ordinary "
         "kitchen with some clutter visible behind. Natural window "
         "light, no fill. Unpolished, shot on a phone. Not studio, "
         "not commercial. Product unchanged.",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

The clauses doing the work:

| Clause | Why |
|---|---|
| "already mid-sentence" | No lead-in. The hook starts immediately |
| "handheld, slight shake" | Gimbal-smooth reads as produced |
| "slightly uneven exposure" | Perfect exposure reads as lit |
| "ordinary kitchen with clutter" | Pristine sets read as sets |
| "natural window light, no fill" | Fill light is a studio signal |
| **"Not studio, not commercial"** | Negative framing. The prior is strong |
| "Product unchanged" | Accuracy doesn't relax with the framing |

That last one matters: **the casual look is a styling choice; product accuracy is not negotiable.** See `product-demo-video`.

Full detail in `ugc-authenticity-signals`.

## The product must be real

Use the actual product photo as a reference. Not a generated approximation.

```
1. Real product photo
2. remove_background        2 credits
3. create_brand_asset       free
4. Reference it in every beat
```

A UGC ad for a product that doesn't quite look like your product is worse than no ad — it's advertising something you don't sell, and customers notice on arrival. See `image-to-video-animation`.

## Where a talking creator is worth it

The hook and the CTA benefit from a face. The middle usually doesn't.

```
Beat 1  HOOK      face, talking          → lip sync, 3 credits/sec
Beat 2  PROBLEM   the mess, b-roll       → cheap
Beat 3  PRODUCT   hands using it, b-roll → cheap
Beat 4  RESULT    the outcome, b-roll    → cheap
Beat 5  CTA       face, talking          → lip sync
```

**Only the face beats need `generate_lip_sync`** at 3 credits/second. Two 3-second talking beats is 18 credits; generating all 18 seconds as talking head is 54. The b-roll beats carry the voiceover at a fraction of the cost. See `lip-sync-spokesperson-video` and `b-roll-generation`.

## Disclosure, and this is not optional

A synthetic creator delivering what reads as a personal testimonial is the single highest-risk output on this platform, and the rules are not ambiguous:

- **Don't present a generated person as a real customer.** That's a false endorsement — an advertising-standards violation in most markets, independent of platform policy
- **Don't attribute experience the person didn't have.** "I've used this for three months" from someone who doesn't exist is a fabricated claim
- **Label AI-generated content** where the platform requires it. TikTok, Meta and others require disclosure of realistic synthetic media and apply their own detection; undisclosed content can have distribution limited
- **Don't use a real person's likeness** without permission, including stock portraits whose licence excludes synthetic use

**What is fine:** a clearly-labelled synthetic presenter demonstrating a product, in the UGC visual register, making claims the product can substantiate.

**What is not:** a synthetic person claiming to be a customer who bought it and loved it.

The distinction is testimony, not aesthetics. See `ugc-disclosure-compliance` and `ugc-legal-likeness-rights`.

## Claim accuracy

A UGC ad is an advertisement, and the informal register doesn't lower the bar.

- **Don't show a result the product doesn't produce**
- **Don't imply a timescale it doesn't achieve**
- **Regulated categories** — health, beauty, supplements, financial — go through the same approval as filmed advertising
- **Before-and-after** imagery is specifically regulated in several categories. See `before-after-transformation-video`

## Variants are the point

The reason to generate rather than commission: you can afford twelve versions.

```
1 creator asset + 1 product asset
6 hook drafts       → test the openings
2 enhanced winners  → cut onto one body
= 6 testable creatives for a fraction of six full renders
```

Hooks carry nearly all the performance variance in short-form. Spend the iteration budget there. See `video-ab-testing-variants` and `ugc-hook-library`.

## Don't

- **Don't render polished, studio-lit video.** It's the default and it's wrong here.
- **Don't open on an establishing shot.** Start mid-action or mid-sentence.
- **Don't generate the whole ad as one clip.** Beats, assembled.
- **Don't lip-sync the whole runtime.** Face beats only.
- **Don't use a generated approximation** of your product.
- **Don't present a synthetic person as a real customer.**
- **Don't skip the AI disclosure.**
- **Don't let the casual register relax claim accuracy.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-authenticity-signals`, `ugc-creator-persona-design`, `ugc-script-writing`, `ugc-hook-library`, `ugc-disclosure-compliance`, `tiktok-video-generation`
