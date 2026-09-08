---
name: social-commerce-product-images
description: Produce product imagery for social feeds and paid social, where native and scroll-stopping beats catalogue-clean. Use whenever the user needs product images for Instagram, Facebook, Pinterest, TikTok Shop, paid social ads, or social commerce placements.
---

# Product Images for Social

A listing image is judged against a spec. A social image is judged against everything else in the feed — and a clean catalogue shot loses to a photograph of a person's kitchen every time.

The whole discipline is producing something that looks like content rather than like a product photo.

## Feed images vs listing images

| | Listing image | Social image |
|---|---|---|
| Background | Pure white, mandated | A real place |
| Job | Identification | **Stop the scroll** |
| Polish | Clean, neutral | Often deliberately less |
| Text | Prohibited on main | Encouraged, overlaid |
| Product size in frame | 85%+ | Can be small, in context |
| Ratio | 1:1 | 1:1, 4:5, 9:16 — several |
| Judged | Against a spec | Against the feed |

**Don't post the listing image to social.** It's the most common mistake and it's immediately identifiable as a product photo, which is the signal to scroll past.

## Ratios: generate native, several of them

Social needs multiple ratios and cropping loses the composition.

| Placement | Ratio | Notes |
|---|---|---|
| Instagram feed | **3:4** (closest to 4:5) | Taller = more feed height |
| Instagram carousel | 1:1 | Safest across the set |
| Instagram grid tile | 1:1 | The crop of your feed post |
| Story / Reels still | **9:16** | Middle third clear for text |
| Facebook feed | 1:1 or 3:4 | |
| Pinterest | **3:4 or 9:16** | Tall performs strongly |
| TikTok Shop | 9:16 | |
| Paid social | 1:1 **and** 3:4 | Generate both natively |

**Pinterest is the outlier worth targeting** — tall images, long content lifespan, and genuine search intent. A 9:16 product image with overlaid text performs there in a way it doesn't elsewhere.

Same cutout, same scene prompt, same seed, different `aspect_ratio`. See `aspect-ratio-strategy`.

## Making it look native

The prompting is the opposite of listing imagery.

```
generate_image(
  model="flux-2-pro",
  reference_images=[PRODUCT_REFS],
  prompt="The same bottle on an ordinary kitchen counter with some "
         "everyday clutter visible — a mug, a folded cloth. Natural "
         "window light, slightly uneven exposure. Shot as if on a "
         "phone, casual framing, product slightly off-centre. "
         "Unpolished, not studio, not commercial product "
         "photography. Product unchanged.",
  aspect_ratio="3:4",
  seed=8812
)
```

The clauses that do the work:

**"Ordinary… with some everyday clutter"** — the model's default is a pristine surface, which reads as a set.

**"Slightly uneven exposure", "casual framing", "slightly off-centre"** — deliberate imperfection.

**"Not studio, not commercial product photography"** — the negative framing. The prior is strong and needs explicit counterweight.

**"Product unchanged"** — the accuracy requirement doesn't relax just because the framing does.

This is the same principle as prompting against polish for TikTok video. See `tiktok-video-generation`.

## Where polish is right

Not every platform wants rough. Choose deliberately rather than defaulting.

| Platform | Register |
|---|---|
| TikTok Shop | **Rough, native, phone-shot** |
| Instagram feed | Considered — good light, clean, intentional |
| Pinterest | Polished, aspirational, styled |
| Facebook feed | Middle. Native performs |
| Paid social | Test both. Often native wins |
| Brand website | Polished |

**Instagram and Pinterest tolerate and often reward polish**; TikTok Shop punishes it. Same product, different prompt per destination.

## Text overlay and the safe area

Text is allowed and usually helps, and it goes in an editor — never in the generation.

- **Prompt a low-detail region** where the text will sit: "generous plain space in the upper third"
- **Story and Reels stills:** middle third only. The platform covers roughly the top 10% and bottom 25%
- **Feed images:** more freedom, but keep text away from the very edges
- **3-6 words.** A social image is read at a glance
- **Never render text in the generation.** It garbles and you'll want variants

See `video-caption-subtitle-planning` — the principles are identical for stills.

## Paid social

When it's an ad rather than organic, several things change.

- **Test native against polished.** Don't assume. Native usually wins on TikTok, less reliably elsewhere
- **Generate several variants and let performance decide.** One variable per variant
- **Higher resolution.** Ad review and larger placements are less forgiving
- **Claim accuracy applies fully.** A generated visual is a product claim, and platforms review ads
- **AI disclosure.** Meta, TikTok and others require disclosure of realistic AI-generated content in ads, and apply their own detection

See `video-ab-testing-variants` for the testing discipline — it applies identically to stills.

## Variant volume is the advantage

The real reason to generate social imagery rather than shoot it: you can afford twelve versions.

```
1 cutout                        2 credits
6 scene variants                3 credits each = 18
3 ratios each of the best 2     generated native
= a full social set for well under 40 credits
```

**Spend the budget on variants, not resolution.** Performance variance between creatives dwarfs the quality difference between HD and 4K on a recompressed feed image. See `video-credit-cost-management`.

## Accuracy still applies

The relaxation is in the framing, not the product.

- **Product unchanged** — shape, label, colour, proportions
- **Colour verified** against the physical product, even under a casual grade
- **No props implying included items** without clarity
- **No implied claim** the product can't support
- **Reference-based, always.** Don't let a casual social prompt reinvent the product

A social image is still an advertisement. See `product-image-qa-review`.

## Don't

- **Don't post the listing image to social.**
- **Don't crop one master into every ratio.** Generate native.
- **Don't default to polish.** Choose per platform.
- **Don't render text into the image.**
- **Don't put text in the bottom quarter** of a 9:16.
- **Don't spend on 4K** for a recompressed feed image. Spend on variants.
- **Don't let the casual framing relax product accuracy.**
- **Don't skip the AI disclosure** on paid social.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lifestyle-product-photography`, `flat-lay-composition`, `seasonal-product-styling`, `aspect-ratio-strategy`, `tiktok-video-generation`, `video-ab-testing-variants`
