---
name: ugc-disclosure-compliance
description: The disclosure and advertising rules for synthetic UGC — AI labelling, false endorsement, testimonial rules and where the hard lines are. Use whenever the user generates creator-style content, synthetic testimonials, AI presenters in ads, or asks what they can legally claim with generated content.
---

# Disclosure and Advertising Rules

Synthetic UGC is the highest-risk output on this platform, because the format's entire persuasive power comes from appearing to be a real person's genuine experience — and that appearance is the thing regulators and platforms are targeting.

This skill is the line. It's worth getting right before you build a campaign on top of it.

**This is not legal advice.** Rules differ by market and are changing quickly. For anything at scale or in a regulated category, get a review in the relevant jurisdiction.

## The two separate obligations

People conflate these and they're different.

**1. Platform AI labelling.** TikTok, Meta, YouTube and others require disclosure of realistic AI-generated content, apply their own detection, and can limit distribution on undisclosed synthetic media. This is a platform policy matter.

**2. Advertising law on endorsements and claims.** Presenting a synthetic person as a real customer is a false endorsement — a consumer protection matter, independent of any platform's policy. Labelling the content as AI does **not** cure a false testimonial.

You need to satisfy both, and satisfying one doesn't satisfy the other.

## The hard lines

**Never:**

- **Present a synthetic person as a real customer.** "I bought this and it changed my routine" from someone who doesn't exist is fabricated testimony
- **Attribute experience the person didn't have** — duration of use, results achieved, comparisons made
- **Use a real person's likeness without permission**, including public figures and stock portraits whose licence excludes AI or synthetic use
- **Imply endorsement** by a real, identifiable individual who hasn't given it
- **Fabricate reviews, ratings or comment screenshots**
- **Generate a "customer" demographic** to imply popularity with a group
- **Show results the product doesn't produce**, in any register

**Fine:**

- A **clearly-labelled synthetic presenter** demonstrating a product
- A **brand spokesperson character**, disclosed, making substantiated claims
- Generated **scenes, settings and b-roll** around accurate product footage
- The **UGC visual aesthetic** — handheld, natural light, ordinary setting
- A **real customer's genuine testimonial**, with permission, in generated scenery

**The distinction is testimony, not aesthetics.** You may use the creator-style look. You may not use it to fabricate someone's experience.

## Why "it's obviously an ad" isn't a defence

A common argument: everyone knows social ads are ads, so nobody is deceived.

It doesn't hold, for two reasons. Advertising standards in most markets treat a testimonial as a specific claim type with its own substantiation requirement — a fabricated one is a breach whether or not the surrounding context is commercial. And UGC-style creative works *precisely because* it doesn't read as an ad, which is the deception being regulated.

If the format's effectiveness depends on the viewer thinking it's a real person's opinion, that's the problem rather than the defence.

## Disclosure that actually works

| Method | Strength |
|---|---|
| Platform's own AI content toggle | **Required where available. Use it** |
| On-screen label in the video | Strong, visible |
| Caption disclosure | Reasonable, often missed |
| Bio or profile-level note | Weak. Not per-content |
| Metadata only | Weak on its own |
| Nothing | Non-compliant, and detectable |

**Use the platform's native AI disclosure toggle where one exists**, plus an on-screen or caption note. Platforms increasingly detect synthetic media independently, and content flagged by detection rather than declared can be down-ranked.

Wording that's clear without being cumbersome: *"AI-generated"*, *"Created with AI"*, *"AI presenter"*. Avoid euphemism — "digitally enhanced" reads as evasion.

## Regulated categories

Where generated UGC needs legal review before publishing, not after.

| Category | The issue |
|---|---|
| **Health, medical, supplements** | Outcome claims, before-and-afters, testimonials all restricted |
| **Beauty and cosmetics** | Result claims and before-and-after imagery regulated |
| **Financial services** | Returns claims, risk warnings, regulated language |
| **Weight loss and fitness** | Body imagery and results heavily restricted |
| **Alcohol** | Depiction rules vary widely by market |
| **Children's products** | Advertising to children heavily restricted |
| **Gambling** | Restricted almost everywhere |

In several of these, **synthetic before-and-after imagery is not permitted at all** in some markets. Check before generating.

## Body representation

An area with active and expanding regulation.

- **Body-altering retouching in advertising requires disclosure** in a growing number of jurisdictions. Generating a body is arguably a stronger version of the same act
- **Don't generate a body type to imply a product result**
- **Don't reshape a generated model** between shots
- Health, diet and fitness categories carry additional restrictions

See `ugc-diversity-representation` and `model-wearing-product`.

## The workable position

For a brand that wants the UGC format without the exposure:

```
1. Generate a presenter. Save them as a character brand asset
2. Give them a consistent, disclosed identity —
   a brand spokesperson, not a customer
3. Have them demonstrate and explain, not testify
4. Make only claims the product can substantiate
5. Label it as AI-generated, on-platform and on-screen
6. Use real customer testimonials separately, with permission
```

**"Here's how this works" is a demonstration. "I've used this for months and it changed everything" is testimony.** The first is fine from a synthetic presenter; the second isn't.

Real testimonials remain available — get them from real customers, with permission, and generate the *scenery* around their footage with `replace_background` if you want production value. That's honest and it's cheap.

## The pre-publish check

```
[ ] Platform AI disclosure applied
[ ] On-screen or caption label present
[ ] No claim of personal experience by a synthetic person
[ ] No implied endorsement by a real identifiable individual
[ ] No fabricated reviews, ratings or comments
[ ] No likeness used without written permission covering synthetic use
[ ] Every product claim substantiated
[ ] No before-and-after in a category that restricts it
[ ] Regulated category → legal sign-off obtained
[ ] Human review completed. Not auto-published
```

## Don't

- **Don't present a synthetic person as a real customer.** The central line.
- **Don't assume an AI label cures a false testimonial.** Two separate obligations.
- **Don't use stock portraits** as references without checking the licence.
- **Don't fabricate reviews, ratings or comment screenshots.**
- **Don't rely on "everyone knows it's an ad."**
- **Don't use euphemistic disclosure.**
- **Don't auto-publish** synthetic creator content.
- **Don't publish in a regulated category** without legal review.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-legal-likeness-rights`, `ugc-testimonial-video`, `ugc-creator-persona-design`, `video-brand-safety-moderation`, `ugc-review-format`, `ugc-diversity-representation`
