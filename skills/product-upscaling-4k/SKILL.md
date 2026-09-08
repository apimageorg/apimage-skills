---
name: product-upscaling-4k
description: Upscale product images with edit_image to meet marketplace resolution and zoom requirements. Use whenever the user has an image that's too small, needs 4K or high-resolution output, mentions upscaling or zoom requirements, or has a soft or low-resolution product photo.
---

# Upscaling to 4K

`edit_image` supports upscaling. APImage generates at SD, HD, Full HD and 4K, and upscaling is how you get an existing image — supplied, legacy, or generated small — up to what a marketplace or a print job needs.

The important limit: **upscaling adds pixels, not information.** It makes a small sharp image into a large sharp image. It does not make a blurry image sharp.

## When you need the resolution

| Use | Minimum | Preferred |
|---|---|---|
| Marketplace main image | 1000px+ | **1600px+ for zoom** |
| Amazon zoom feature | 1600px on the longest side | 2000px+ |
| Print and packaging | 300dpi at final size | 4K master |
| Web hero | Full HD | 4K if it'll be cropped |
| Social | HD | Full HD |
| Master for cropping into ratios | — | **4K** |

**Zoom is the one that catches people.** Several marketplaces only enable their zoom viewer above a threshold, and listings with zoom convert better. An image at 999px doesn't just look smaller — it silently disables a conversion feature.

## The call

```
edit_image(
  image="<source image>",
  prompt="Upscale to 4K. Preserve the product exactly — same shape, "
         "label, colour and proportions. Increase detail in fabric "
         "texture and label print. No changes to composition or "
         "lighting."
)
```

Two clauses that matter:

**"Preserve the product exactly."** Upscaling models invent detail, and invented detail on a product is a misrepresentation. Constrain it.

**Name where detail should increase** — texture, print, edges. That directs the added detail toward the real surfaces rather than letting it hallucinate broadly.

## What upscaling can and can't fix

| Fixable | Not fixable |
|---|---|
| Small but sharp source | Out-of-focus source |
| Slight softness | Motion blur |
| Meeting a pixel-count threshold | Blown highlights with no data |
| Adding plausible surface texture | Crushed blacks with no data |
| Cleaning mild JPEG artefacts | Heavy compression damage |
| — | **Illegible text** |

**Text is the important one.** Upscaling illegible label text produces *differently* illegible text — the model invents letterforms that look like text and aren't your copy. For anything with legible packaging text that has to be right, the answer is a photograph or an overlay, not an upscale. See `packaging-mockup-generation`.

## Generate large rather than upscaling

The better strategy where you have the choice. Generate at the resolution you need, once.

```
Better:   generate_image at 4K                → real detail
Worse:    generate_image at SD, then upscale  → invented detail
```

Upscaling is for images you didn't generate — supplied product photography, legacy assets, a client's catalogue. For new generations, set the resolution at generation time.

The practical workflow:

```
1. Iterate at SD or HD        cheap, fast
2. Lock the seed and prompt
3. Regenerate at 4K           same seed, same prompt, full resolution
```

Same seed and prompt at a higher resolution gives you real detail rather than upsampled detail, and it costs one generation. See `seed-locked-iteration` and `image-model-selection`.

## Check what the upscaler invented

The QA pass specific to upscaling, and it's necessary rather than optional.

```
[ ] Zoom to 100% and compare against the source side by side
[ ] Label text: still correct, or now invented letterforms?
[ ] Product texture: plausible, or fabricated pattern?
[ ] Logo shape: exact, or re-drawn?
[ ] Edges: clean, or over-sharpened with halos?
[ ] Fine detail: real, or smeared/waxy?
[ ] Skin, if present: natural, or plastic?
```

**Over-sharpening halos and waxy texture are the two tells** that an image has been upscaled aggressively. Both read as low quality even though the pixel count went up.

`analyze_image` (1 credit) on the upscaled version will read out any text it sees — a quick way to catch invented label copy.

## Cost and sequence

`edit_image` bills generation credits. Sequence matters for cost:

```
Right:  edit → retouch → relight → upscale last
Wrong:  upscale first → then edit at 4K
```

**Upscale last.** Editing at 4K costs more and is slower for no benefit — do the retouching, relighting and background work at working resolution, then upscale the finished image once.

The exception is inpainting fine detail, where you sometimes need the resolution to work at. See `inpainting-product-fixes`.

## Downscaling from a 4K master

The complement, and it's free. Generate or upscale one 4K master, then derive every other size and ratio from it in an editor.

```
4K master (1:1)
  → 2000px  marketplace main
  → 1080px  social
  → 800px   listing gallery
  → crops   4:5, 9:16, 16:9 for other placements
```

Downscaling always looks better than upscaling. One 4K master is a better investment than five images generated at their target sizes. See `marketplace-image-compliance`.

## Don't

- **Don't upscale a blurry or out-of-focus image.** Nothing to recover.
- **Don't upscale illegible text** and ship it. It gets invented.
- **Don't upscale when you could regenerate at 4K.** Real detail beats added detail.
- **Don't upscale before editing.** Upscale last.
- **Don't skip the side-by-side check** against the source.
- **Don't accept over-sharpening halos** or waxy texture.
- **Don't ship under the marketplace zoom threshold.** It disables a conversion feature.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-model-selection`, `inpainting-product-fixes`, `white-background-ecommerce`, `marketplace-image-compliance`, `product-image-qa-review`, `product-detail-macro`
