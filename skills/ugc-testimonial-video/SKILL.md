---
name: ugc-testimonial-video
description: Produce testimonial-format video legitimately — using real customer words with generated scenery, or a clearly disclosed spokesperson. Use whenever the user wants testimonial video, social proof creative, review-style ads, or asks a generated presenter to describe a personal experience.
---

# Testimonial Video

Testimonial is the highest-converting UGC format and the one with the least room to improvise. A testimonial is a claim about a real person's real experience, and generating the person doesn't generate the experience.

There is a legitimate way to produce this format at scale. It's just not "have an AI person say they love it."

## The line

| Approach | Legitimate |
|---|---|
| Real customer's words, on screen as text, over generated b-roll | **Yes** |
| Real customer's audio, over generated b-roll | **Yes**, with permission |
| Real customer's footage, generated scenery extending it | **Yes**, with permission |
| Disclosed spokesperson explaining what the product does | **Yes**. But it isn't a testimonial |
| Generated person saying "I've used this for three months" | **No** |
| Generated person implying they're a customer | **No** |

**The rule in one sentence:** the *words* must come from someone who actually had the experience. Everything visual around them can be generated.

That's not a limitation on the format — it's a reallocation of effort. Collecting one genuine review is cheap. Turning it into fifteen pieces of video is what generation is good at.

## The workflow that works

Real words. Generated everything else.

```
1. Collect a real review          email, support ticket, marketplace review
2. Get written permission         to use it in advertising
3. Extract the strongest lines    verbatim. Don't improve them
4. Generate the scenery           the product, the setting, the result
5. Put the words on screen        as text, attributed
6. Assemble
```

The on-screen-text testimonial is a native, well-understood format on TikTok and Reels, and it's the cleanest solution to the whole problem: the claim is genuine and attributed, the visuals are generated, and nobody is pretending to be a customer.

## Generating the scenery

The b-roll a testimonial needs is exactly what generation does well — no faces, no claims, no identity.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[PRODUCT_ASSET],
  prompt="Close on the bottle on a kitchen worktop, a hand enters "
         "frame and picks it up. Handheld phone footage, slight "
         "shake, natural window light, ordinary kitchen with some "
         "clutter. Not studio, not commercial.",
  aspect_ratio="9:16", resolution="hd", duration=5, seed=4271
)
```

Hands, product, setting, result. **No presenter, no dialogue, no lip sync** — which also makes it far cheaper than a talking-head build. See `b-roll-generation`.

## The spokesperson alternative

If you have no reviews to work from, the honest version of the format is a **disclosed spokesperson demonstrating**, not a fictional customer testifying.

```
Not a testimonial:  "Here's what this does. Coffee rings on oak
                     aren't stains — it's the finish lifting. This
                     puts it back."

A false testimonial: "I've had this three months and my table looks
                     brand new."
```

Both can be delivered by the same generated presenter in the same handheld kitchen clip. The first is a demonstration; the second is fabricated endorsement. The visual register is identical — the difference is entirely in the script. See `ugc-script-writing` and `ugc-disclosure-compliance`.

## Why this matters commercially, not just legally

Advertising rules across most markets treat a fabricated endorsement as a misleading practice — separate from and unaffected by an AI-content label. A platform label says "this was made with AI"; it doesn't say "this person didn't buy the product."

But the practical exposure usually isn't a regulator:

- **Platform enforcement.** Ad accounts get restricted for misrepresentation, and appeals are slow
- **A screenshot.** "This brand invented a customer" is a cheap and durable attack
- **Review-platform terms.** Fabricated reviews breach them outright, and some jurisdictions treat them as a specific offence

A genuine review plus generated b-roll gets the same conversion mechanism with none of this.

## Getting reviews worth using

The input is the constraint, so make collecting it deliberate.

- **Ask a specific question.** "What did it replace?" and "What almost stopped you buying it?" produce usable lines. "Any feedback?" produces "great product"
- **Ask soon after the result**, not at purchase
- **Ask permission in the same message**, in writing, naming advertising as the use
- **Keep the verbatim.** Log the original text, the date and the permission alongside every clip you build
- **Don't tidy the language.** The imperfection is the credibility

**One good review supports many creatives.** Different lines, different b-roll, different hooks. See `ugc-batch-testing`.

## Editing a real quote

You will want to shorten. There's a safe way and an unsafe way.

| Change | Safe |
|---|---|
| Trimming for length | Yes, if the meaning survives |
| Cutting filler words | Yes |
| Fixing an obvious typo | Yes |
| Cutting a reservation | **No.** That's changing the review |
| Strengthening a claim | **No** |
| Combining two reviewers into one quote | **No** |
| Adding a claim they didn't make | **No** |

**Keep the reservation.** "It's expensive but worth it" outperforms "it's worth it" — and cutting the first half is the edit that turns a quote into a misrepresentation.

## Assembling the piece

```
0-3s     Hook: the problem, in the customer's words. Text on screen
3-8s     Generated b-roll: the product in use, real setting
8-13s    The result. Generated. Text continues
13-16s   Attribution: first name, and where the review came from
16-18s   CTA
```

The attribution frame is worth the second it costs — it's the difference between a claim and a sourced claim, and it reads as more confident, not less.

## Don't

- **Don't have a generated presenter claim personal experience.** Ever.
- **Don't invent a customer**, a name, or a result.
- **Don't fabricate reviews** on review platforms. Separate and worse.
- **Don't cut the reservation** out of a real quote.
- **Don't merge multiple reviewers** into one voice.
- **Don't rely on an AI label** to cure a false endorsement. Different obligations.
- **Don't skip written permission**, even for a public review.
- **Don't tidy the phrasing.** Rough language is the credibility.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-disclosure-compliance`, `ugc-review-format`, `ugc-script-writing`, `b-roll-generation`, `ugc-legal-likeness-rights`, `ugc-ad-video-generation`
