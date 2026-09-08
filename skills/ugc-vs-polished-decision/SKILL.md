---
name: ugc-vs-polished-decision
description: Decide when UGC-style creative beats polished brand creative and when it doesn't, per platform, category and funnel stage. Use whenever the user is choosing a creative register, asks whether to go rough or polished, or is defaulting to one style everywhere.
---

# UGC or Polished

UGC-style creative outperforms polished brand video on some platforms, in some categories, at some funnel stages. Defaulting to rough everywhere is as much a mistake as defaulting to polished — it's just a more fashionable one.

This is the decision framework.

## By platform

| Platform | Register | Confidence |
|---|---|---|
| TikTok | **UGC** | High. Polish is penalised |
| TikTok Shop | **UGC** | High |
| Instagram Reels | Either. Considered polish is fine | Medium |
| Instagram feed | **Polished** | Medium-high |
| Instagram Stories | UGC | Medium |
| Pinterest | **Polished, aspirational** | High |
| YouTube Shorts | Clear and informational over either | Medium |
| Facebook feed | UGC | Medium |
| LinkedIn | Polished, but not glossy | Medium |
| Paid social | **Test both** | — |
| Brand website | **Polished** | High |
| Email | Polished | Medium |
| Retail and trade | Polished | High |

**Pinterest is the clearest counter-example to the UGC default.** It's an aspirational, visually-curated platform where rough content underperforms. A styled, well-lit product image with overlaid text is the right creative there.

**Instagram feed and Reels differ from each other.** Same platform, different register — Reels tolerates roughness, the feed grid rewards a considered look. See `instagram-reels-generation`.

## By category

| Category | Register | Why |
|---|---|---|
| Household, cleaning | **UGC** | Demonstration in a real home is the proof |
| Beauty, skincare | **UGC** for social, polished for brand | Application is the content |
| Food, drink | Either. Appetite appeal often wants polish | |
| Fashion, apparel | Polished for brand, UGC for fit and styling | |
| Consumer electronics | UGC for demos, polished for hero | |
| Luxury | **Polished. Firmly** | Rough undermines the premium claim |
| Jewellery, watches | **Polished** | The product is the craft |
| Furniture, homeware | Polished for hero, UGC for in-situ | |
| B2B software | Polished, but plain | Rough reads as unprofessional |
| Financial services | **Polished** | Trust and regulation both |
| Healthcare | **Polished** | Credibility requirement |
| Trades and tools | **UGC** | Site footage is the credential |

**Luxury and regulated categories are where UGC actively hurts.** A handheld, uneven-exposure clip of a premium watch undermines the thing being sold. A rough financial services ad reads as untrustworthy — and in a regulated category, credibility is part of compliance.

## By funnel stage

| Stage | Register |
|---|---|
| Cold, top of funnel | **UGC.** It has to not look like an ad to get watched |
| Retargeting | Either. Test |
| Consideration | Polished. They're evaluating, and quality signals matter |
| Product page | **Polished** |
| Post-purchase | UGC. Community register |

**The clearest pattern in this whole skill:** UGC for cold acquisition, polished for consideration. Cold audiences skip anything that looks like an ad; warm audiences are evaluating and read production quality as a proxy for company quality.

## By what the creative has to do

Sometimes the job settles it regardless of platform.

| Job | Register |
|---|---|
| Demonstrate a use | **UGC.** Real hands, real setting |
| Show a transformation | UGC. Credibility matters |
| Establish premium positioning | **Polished** |
| Communicate a spec | Polished. Clarity over vibe |
| Build trust in a considered purchase | Polished |
| Feel like a recommendation | **UGC** |
| Show scale or fit | Either, but must be clear |
| Explain something complex | Clear beats both |

## Same product, two prompts

The practical upshot: the register is a prompt decision, not a production decision. Same references, same product, opposite instructions.

```python
UGC = ("Handheld phone footage, slight shake, slightly uneven "
       "exposure, ordinary kitchen with clutter visible, natural "
       "window light with no fill, subject slightly off-centre. "
       "Unpolished, shot on a phone. Not studio, not commercial.")

POLISHED = ("Locked camera on a tripod, even exposure, soft "
            "wraparound studio light with gentle falloff, clean "
            "uncluttered surface, subject centred, considered "
            "composition, warm neutral grade, shallow depth of field.")
```

Both from the same product asset, at a few credits each. There's no cost argument for guessing — generate both and test.

## Test, don't theorise

For paid, the honest answer is that this is testable and cheap to test.

```
2 registers × 3 hooks = 6 drafts
enhance the 2 best     → run both
```

Drafting makes a register test nearly free, and internal preference is a poor predictor. See `video-ab-testing-variants` and `draft-then-enhance-workflow`.

**What people get wrong when they theorise:** brand teams assume polished; growth teams assume UGC. Both are sometimes wrong, and the data settles it in a week.

## The hybrid that often wins

Not a binary. A common high-performing structure:

```
0-3s     UGC hook       handheld, real person, real setting
3-12s    Polished       clean product demonstration, good light
12-16s   UGC result     back to the real setting
16-18s   Clean CTA      simple, readable
```

The UGC opening gets it watched; the polished middle communicates the product clearly; the UGC close keeps it credible. Generate the beats separately and cut — which is how you'd build it anyway. See `multi-scene-video-assembly`.

## Don't

- **Don't default to UGC everywhere.** Pinterest, luxury and regulated categories punish it.
- **Don't default to polished everywhere.** Cold social skips it.
- **Don't use rough creative for luxury.** It undermines the positioning.
- **Don't use rough creative in regulated categories.** Credibility is part of compliance.
- **Don't theorise when you can test.** Drafting makes it cheap.
- **Don't treat it as binary.** The hybrid structure often wins.
- **Don't assume Reels and the Instagram feed want the same thing.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-authenticity-signals`, `tiktok-video-generation`, `instagram-reels-generation`, `video-ab-testing-variants`, `social-commerce-product-images`, `ugc-ad-video-generation`
