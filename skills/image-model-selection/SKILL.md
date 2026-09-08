---
name: image-model-selection
description: Choose between APImage's image models — flux-2-klein-9b, flux-2-pro, flux-2-max, gpt-image-2 and dall-e-3 — and the resolution to pair with each. Use whenever the user is generating images with APImage, asks which model to use, mentions credit cost on images, or is getting output that isn't good enough.
---

# Choosing an Image Model

Five models across two endpoints, priced 1-9 credits depending on model and resolution. The decision is mostly about where you are in the loop and whether the output has text or a specific product in it.

## The models

**Image Studio** (`/image-studio`) — generation and editing:

| Model | Use for |
|---|---|
| `flux-2-klein-9b` | Cheapest. Volume drafts, composition exploration |
| `flux-2-pro` | **Default.** The right answer for most work |
| `flux-2-max` | Highest fidelity. Hero shots, print, anything going behind ad spend |
| `gpt-image-2` | Also available here |

**OpenAI Images** (`/ai-image-generate`):

| Model | Use for |
|---|---|
| `gpt-image-2` | **Default here.** Strong prompt adherence, better at text |
| `dall-e-3` | Legacy. Rarely the right choice now |

## The two decisions that actually matter

**1. Is there text in the image?**

FLUX models render text poorly — garbled, misspelled, inconsistent. `gpt-image-2` is materially better at it.

So: packaging with legible copy, signage, a label you need readable, a mockup with real words — use `gpt-image-2`. Everything else, FLUX.

Even then: **if the text has to be exactly right, overlay it in an editor.** No model is reliable enough for a product label you'll ship. See `packaging-mockup-generation`.

**2. Where are you in the loop?**

```
Explore composition   flux-2-klein-9b   cheap, many
Iterate the look      flux-2-pro        default
Final hero            flux-2-max        once
```

Exploring at `flux-2-max` is the image equivalent of iterating video at full quality — it works and it costs several times what it needed to. See `video-credit-cost-management`.

## Resolution

SD, HD, Full HD and 4K are supported, and resolution scales cost.

| Use | Resolution |
|---|---|
| Composition drafts | SD |
| Social and web | HD |
| Marketplace listings | Full HD |
| Hero, print, or anything to be cropped into | 4K |
| Reference for video generation | HD is fine |

**Match resolution to destination.** A 4K render for an Instagram post is wasted — the platform recompresses it to a fraction of that. Spend the difference on more variants.

The exception: **anything you'll crop into wants headroom.** A 4K master crops to several aspect ratios without softening; an HD one doesn't. See `marketplace-image-compliance`.

## Aspect ratios

**1:1, 16:9, 9:16, 4:3, 3:4, 21:9, 9:21.**

Generate at the target ratio rather than cropping. A 1:1 listing image cropped from 16:9 loses a third of the composition and was never framed for the square.

| Destination | Ratio |
|---|---|
| Marketplace listing | 1:1 |
| Shopify product | 1:1 or 4:3 |
| Social feed | 1:1 or 4:5 (use 3:4) |
| Story / Reels cover | 9:16 |
| Web hero | 16:9 or 21:9 |
| Email | 16:9 or 1:1 |

## Batching on the OpenAI endpoint

`num_images` accepts **1-4** on `/ai-image-generate`, which returns several variations in one call. Useful for composition exploration — four options for one request.

Note the different rate limits: **60/minute** on `/ai-image-generate`, **30/minute** on the background tools, **120/minute** on `/generations` and `/brand-assets`.

## Seeds and references work across the choice

Two things to know about how the parameters interact with the model choice:

- **Seeds don't transfer between models.** A seed on `flux-2-pro` means nothing on `gpt-image-2`. Lock the model before locking the seed
- **`reference_images` accepts up to 4** for images. Use them whenever the subject must be accurate rather than approximate. See `product-photo-from-reference`

## `enhance_prompt` is free

No reason not to run it on a thin prompt before generating.

```
enhance_prompt(prompt="serum bottle on marble, nice light")
→ a fuller specification with lighting, lens and composition filled in
```

Edit its output rather than accepting it — it will sometimes add elements you don't want. Treat it as a first draft that surfaces what you forgot to specify.

## The editing tools are a separate decision

`edit_image` handles enhancement, relighting, colour grading, camera angle adjustment, upscaling, inpainting and erasing via masked regions. Often the right move is **generate once, then edit** rather than regenerating with a modified prompt.

```
Regenerate:  new roll of the dice, composition changes
edit_image:  the same image, one thing changed
```

For "same shot, warmer light" or "same shot, higher resolution", `edit_image` preserves what you liked. See `product-relighting` and `product-upscaling-4k`.

Dedicated background tools are cheaper and better than prompting for a background change:

- `remove_background` — 2 credits, transparent PNG or WebP
- `replace_background` — 3 credits, swaps the scene **and relights the subject**

## Cost discipline

- Explore on `flux-2-klein-9b`, finish on `flux-2-pro` or `flux-2-max`
- Drop resolution while iterating
- `enhance_prompt`, polling, history and brand assets are all **free**
- Use `edit_image` rather than regenerating when you want one thing changed
- Use the dedicated background tools rather than prompting scene swaps
- `check_credits` before a batch

## Don't

- **Don't explore at `flux-2-max`.**
- **Don't use FLUX for legible text.** Use `gpt-image-2`, or overlay it.
- **Don't trust any model for text that must be exactly right.**
- **Don't render 4K for social.**
- **Don't crop to reach an aspect ratio.** Generate native.
- **Don't carry a seed across models.**
- **Don't prompt for a background swap** when `replace_background` does it for 3 credits.
- **Don't regenerate** when `edit_image` will change the one thing.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-from-reference`, `background-replacement-scenes`, `product-relighting`, `product-upscaling-4k`, `seed-locked-iteration`, `video-credit-cost-management`
