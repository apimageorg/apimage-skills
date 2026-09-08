---
name: background-removal-workflow
description: Cut products out of their background with remove_background, producing transparent PNG or WebP assets that feed every other workflow. Use whenever the user needs a transparent cutout, wants to remove a background, needs a product on white, or is preparing a product image for reuse across scenes.
---

# Background Removal

`remove_background` produces a transparent PNG or WebP cutout for **2 credits**. It's the cheapest genuinely useful operation on the platform and the one that makes everything downstream easier.

A clean cutout is the master asset: from it you get white-background listing images, lifestyle composites, video references, mockups and ad creative, without re-cutting each time.

## The call

```
remove_background(
  image="<product photo>"
)
→ transparent PNG or WebP
```

2 credits. That's it — no prompt, no model choice, no iteration loop.

## Why it's the first step, not a last resort

| Downstream use | Why the cutout helps |
|---|---|
| White-background listing images | Composite onto pure white with a controlled shadow |
| `replace_background` scenes | Cleaner subject separation, better relighting |
| Video `reference_images` | Model isn't distracted by the original scene |
| Multi-product composites | Products can be arranged freely |
| Packaging mockups | Clean edges against any surface |
| Ad creative and social | Product over brand colour, text, gradient |
| Email and web | Transparent over any page background |

**Cut out first, then decide the background.** Doing it the other way — generating scenes from a photo with its original background still in it — gives the model competing context and produces worse separation.

## PNG or WebP

| | PNG | WebP |
|---|---|---|
| Transparency | Yes | Yes |
| File size | Larger | **Considerably smaller** |
| Marketplace acceptance | **Universal** | Varies. Check |
| Editor support | Universal | Good, occasionally awkward |
| Web delivery | Fine | **Better** |

**PNG for anything going to a marketplace or an editor. WebP for web delivery.** Amazon, Etsy and most marketplaces specify accepted formats and PNG is always among them; WebP sometimes isn't. See `marketplace-image-compliance`.

## What cuts out cleanly, and what doesn't

| Clean | Difficult |
|---|---|
| Solid opaque products, defined edges | Transparent glass and bottles |
| Boxes, tins, bottles with labels | Fine hair, fur, feathers |
| Products on plain backgrounds | Mesh, lace, netting |
| High contrast subject vs background | Products matching the background colour |
| Sharp focus throughout | Motion blur or shallow-focus edges |
| Hard, matte surfaces | Mirrors and chrome reflecting the scene |

**Transparent and reflective products are the hard cases**, and they're hard for a reason — the "background" is visible *through* and *in* the product, so there's no clean boundary to find.

For those: cut out anyway, then inspect and repair the edges with `edit_image` inpainting rather than accepting the result. See `inpainting-product-fixes` and `jewelry-reflective-products`.

## Inspect the edges, every time

The step people skip because the thumbnail looks fine.

```
[ ] Zoom to 200% and trace the full outline
[ ] Check for a halo — a light or dark fringe from the old background
[ ] Check fine detail: handles, straps, spouts, thin parts
[ ] Check semi-transparent areas — did they go opaque or vanish?
[ ] Check the shadow: removed entirely, or a grey smear left behind?
[ ] Composite onto BOTH white and black and look again
```

**The white-and-black test is the one that matters.** A halo invisible against white is obvious against black, and vice versa. Any cutout you'll reuse should pass both.

`analyze_image` (1 credit) on the cutout will sometimes name an artefact you'd skimmed past — useful as a second pass on a batch.

## Shadows

`remove_background` removes the background, which includes the contact shadow. That's usually right, and it leaves the product looking like it's floating.

Two fixes:

**Add a shadow in the editor.** A soft contact shadow under the product grounds it. Controllable, consistent across a set, and it's what most marketplace-compliant white-background images use.

**Use `replace_background` instead.** It swaps the scene *and relights the subject to match*, which produces a physically plausible shadow. 3 credits. Better when you want a real scene rather than white. See `background-replacement-scenes`.

For a listing image, the editor shadow is usually the right call — marketplaces often specify pure white and a controlled shadow is easier to keep consistent across a catalogue.

## Save the cutout as the master

```
create_brand_asset(type="product", ...)     # free
```

The cutout is a better brand asset than the original photo, because it composites into anything. Save it, and every subsequent shot — image or video — starts from a clean subject.

Brand asset operations are free. See `product-photo-consistency`.

## Batching

Rate limit on the background tools is **30 requests per minute**. For a catalogue:

```python
import time

for path in product_images:
    cutout = remove_background(image=path)
    save(cutout)
    time.sleep(2.5)          # stay under 30/min
```

At 2 credits each, a 200-product catalogue is 400 credits — worth checking against your quota first with `check_credits`. See `product-photo-batch-pipeline`.

## When not to remove the background

- **The original scene is the shot.** A good lifestyle photo doesn't need cutting out
- **The product needs its context** to read at all — scale references, in-situ furniture
- **The reference is already on plain white** and clean enough
- **Reflective or transparent products** where the cutout will be worse than the original

That last one is a real judgement call. For a glass bottle on a clean surface, a careful `replace_background` often beats a cutout with damaged edges.

## Don't

- **Don't judge the cutout from a thumbnail.** Zoom to 200%.
- **Don't skip the white-and-black test.**
- **Don't use WebP for marketplace uploads** without checking the spec.
- **Don't leave the product floating.** Add a shadow or use `replace_background`.
- **Don't cut out a transparent product** and accept whatever comes back.
- **Don't re-cut the same product per project.** Save the cutout as a brand asset.
- **Don't fire a catalogue at once.** 30/minute on the background tools.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-replacement-scenes`, `white-background-ecommerce`, `inpainting-product-fixes`, `product-photo-from-reference`, `jewelry-reflective-products`, `product-photo-batch-pipeline`
