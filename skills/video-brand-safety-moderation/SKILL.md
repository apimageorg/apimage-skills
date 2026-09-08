---
name: video-brand-safety-moderation
description: Use safety_tolerance and review process to keep generated video and images brand-safe, and handle moderation rejections. Use whenever the user is generating content that will run as paid media, mentions safety_tolerance or moderation or content rejection, gets a 422 error, or asks how to review AI output before publishing.
---

# Brand Safety and Moderation

Generated media that runs behind ad spend, on a brand account, or in front of customers is a brand risk in a way that internal experimentation isn't. Two separate mechanisms matter: the platform's own moderation on generation, and your review before publishing.

The second is the one that actually protects you.

## `safety_tolerance`

APImage exposes `safety_tolerance` on generation, **0 to 4**. Lower is stricter.

```
generate_video(
  ...,
  safety_tolerance=1
)
```

| Setting | Behaviour | Use for |
|---|---|---|
| 0-1 | Strictest. Rejects more prompts | Paid media, regulated categories, brand accounts |
| 2 | Balanced | Most commercial work |
| 3-4 | Permissive. Fewer rejections | Internal exploration, creative latitude |

**Set it strict for anything going behind ad spend.** A rejection at generation costs you a retry. An unsafe frame discovered after the campaign is live costs considerably more, and platform ad review is less forgiving than generation moderation.

The trade is real: strict settings reject prompts that were fine. That's the correct direction for commercial output.

## Handling a rejection

Moderation rejections come back as an MCP tool error carrying the upstream HTTP status — typically **422**.

**A 422 is deterministic.** The same prompt fails the same way. Retrying unchanged produces an identical failure, so don't loop on it.

What to do instead:

1. **Read which part triggered it.** Usually a specific phrase, not the whole prompt
2. **Rewrite the trigger**, don't just soften it. "Attractive young woman in minimal clothing" fails; "woman in a linen shirt, mid-thirties" is a different prompt with a different intent
3. **Check the reference images.** A rejection can come from an input, not the prompt
4. **Raise `safety_tolerance` only if the content genuinely is fine** and the moderation was over-eager — not as a way around a correct rejection

That last distinction matters. If the moderation is right, raising the tolerance to get past it produces content you shouldn't publish.

## What generation moderation doesn't catch

This is the important part. Passing moderation is not the same as being brand-safe. Moderation checks for policy violations; it does not check whether the output embarrasses you.

Things that pass moderation and still shouldn't ship:

- **Warped hands, extra fingers, wrong joint counts** — the classic tell, and it reads as cheap
- **Garbled text** in-frame, on packaging or signage
- **A distorted product label or logo** — worse than no product shot
- **Uncanny faces**, especially at high motion
- **Anatomically impossible motion** that looks fine in a still and wrong in playback
- **Unintended objects** the model added — a second product, a stray hand, an implausible background item
- **Accidental resemblance** to a real person or a competitor's branding
- **Cultural or contextual mismatch** the prompt didn't anticipate

None of those are policy failures. All of them are brand failures.

## The review pass

Before anything publishes, especially before it runs as paid media:

```
[ ] Watch full-length at 100%, sound on and sound off
[ ] Step through the high-motion frames specifically — that's where warping is
[ ] Check hands, faces and any text in frame
[ ] Check the product: label legible, logo undistorted, shape correct
[ ] Check the background for objects you didn't ask for
[ ] Check for accidental likeness to a real person
[ ] Check the claim: does the visual support what the copy says?
[ ] Confirm the AI disclosure is present where required
```

**Step through the high-motion frames.** Warping happens where motion is greatest, and it's invisible in a still and in a casual watch. Scrub frame by frame through the fastest part.

`analyze_image` (1 credit) can help — describe a frame and ask what's in it. It'll sometimes name an artefact or an object you'd missed. Useful as a second pass on a batch, not as a replacement for looking.

## Claim accuracy

The brand risk that has legal consequences rather than aesthetic ones.

- **Don't generate a visual that demonstrates something the product doesn't do.** A generated clip showing a cleaning product removing a stain instantly, when it doesn't, is a false advertising claim regardless of how it was produced
- **Don't generate before-and-after imagery** implying results the product doesn't deliver. This is regulated in health, beauty and fitness categories in most markets
- **Don't generate a scene implying an endorsement** that doesn't exist
- **Don't generate a synthetic person presented as a real customer.** See ugc disclosure compliance

Generated media doesn't get a lower standard for claim substantiation. If anything the ease of producing a persuasive false visual makes the discipline more important, not less.

## Regulated categories

Where the bar rises and generated content needs legal review before publishing, not after:

- **Health, medical and supplements** — outcome claims, before-and-afters
- **Financial services** — returns, risk warnings, regulated language
- **Alcohol** — depiction rules vary widely by market
- **Children's products** — advertising to children is heavily restricted
- **Cosmetics and beauty** — before-and-after and result claims
- **Gambling** — restricted almost everywhere

If the category is regulated, the generated asset goes through the same approval as a filmed one. Keep approval mode on any automated pipeline. 

## Rights and likeness

- **Don't use a real person's face** as a reference without permission — including public figures and stock portraits whose licence excludes synthetic use
- **Don't generate a recognisable competitor's product or branding**
- **Don't reproduce copyrighted characters or protected designs**
- **Generate a synthetic presenter instead.** Cheap, consistent, and unambiguously yours. See ai avatar presenter

## Don't

- **Don't raise `safety_tolerance` to get past a correct rejection.**
- **Don't retry a 422 unchanged.** It's deterministic.
- **Don't treat passing moderation as brand-safe.** They're different checks.
- **Don't skip the frame-by-frame pass on high-motion sections.**
- **Don't publish generated media in a regulated category** without the normal approval.
- **Don't generate a visual claim the product can't substantiate.**
- **Don't use a real person's likeness without permission.**
- **Don't auto-publish.** Keep a human review step on anything customer-facing.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`async-video-job-orchestration`, `ugc-disclosure-compliance`
