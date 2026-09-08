---
name: packaging-mockup-generation
description: Generate packaging mockups and label visualisations, and handle the fact that models cannot render text reliably. Use whenever the user wants a packaging mockup, a label visualisation, a box or bottle or pouch render, or to see artwork applied to a product.
---

# Packaging Mockups

Packaging is the hardest thing to generate, for one reason: **packaging is mostly text, and models render text badly.**

That doesn't make mockups impossible. It means the workflow is different from every other product image: generate the *form*, apply the *artwork* separately.

## The rule

**Never let a model generate your packaging copy.**

It produces text that looks like text — right typeface weight, right layout, plausible letterforms — and reads as gibberish or, worse, as almost-correct wrong words. On a product mockup that's:

- Embarrassing if anyone reads it
- A compliance failure if it's replacing an ingredients list, allergen declaration or warning
- Useless as an approval asset, because nobody can sign off copy that isn't the copy

## The workflow that works

```
1. Generate or photograph the blank form
     — the bottle, box, pouch, tube, unlabelled
2. create_brand_asset(type="product")     free — reusable blank
3. Apply the real artwork in an editor
     — as a warped overlay following the form's surface
4. edit_image relight to integrate it
     — so the label picks up the scene's light
```

The artwork comes from your design files, at full fidelity, with the real copy. The generation supplies the object it sits on.

## Generating the blank form

```
generate_image(
  model="flux-2-pro",
  prompt="A blank matte white 250ml cosmetic pump bottle, no label, "
         "no text, no markings. Standing upright on pure white, "
         "straight on, soft even studio light from above and "
         "slightly left, subtle contact shadow. Clean product "
         "photography.",
  aspect_ratio="1:1",
  seed=8812
)
```

**"No label, no text, no markings" is the important clause.** Left unstated the model adds decorative pseudo-text, which then has to be masked out before you can apply real artwork.

Generate the form once per pack format, save it as a brand asset, and reuse it for every SKU that uses that pack. A cosmetics line with one bottle shape needs one blank.

## If you must generate text

Where the text is decorative rather than informational — a hero shot where the label reads as a shape rather than as copy:

**Use `gpt-image-2`.** It's materially better at text than the FLUX models. On the `/ai-image-generate` endpoint it's the default; it's also available in Image Studio.

```
generate_image(
  model="gpt-image-2",
  prompt="A cosmetic bottle with a minimal label reading exactly "
         "'CALM' in a light sans-serif, centred, nothing else on "
         "the label. Pure white background, soft studio light.",
  aspect_ratio="1:1"
)
```

Even then: **one short word, verified letter by letter, and only where it's decorative.** Any longer copy, and definitely any regulatory copy, gets overlaid.

See `image-model-selection`.

## What must always be overlaid or photographed

Non-negotiable, because these are legally required and wrong versions are a compliance failure:

- Ingredients lists
- Allergen declarations
- Nutrition panels
- Net weight and volume
- Batch codes and dates
- Warnings and safety text
- Certification marks and regulatory logos
- Barcodes
- Country of origin
- Recycling and disposal marks

**Photograph the real pack for any face carrying these.** Generate the marketing angles; photograph the regulatory ones. See `marketplace-image-compliance` and `food-beverage-photography`.

## Applying artwork convincingly

The integration is what separates a mockup from a flat paste-on.

- **Warp the artwork to the form.** A cylindrical bottle curves the label; a flat paste-on reads as fake immediately
- **Match the perspective** of the form
- **Relight after applying.** `edit_image` can integrate the label into the scene's light so highlights and shadows fall across it
- **Add the surface character** — a matte label shouldn't be glossier than the bottle
- **Respect the label's real position and size.** A mockup showing the label larger than it is misrepresents the pack

```
edit_image(
  image=MOCKUP_WITH_ARTWORK,
  prompt="Integrate the label into the scene lighting — the same "
         "soft light from above left falling across the label, "
         "matching highlight and shadow, matte label finish. "
         "Do not change the label artwork or text."
)
```

**"Do not change the label artwork or text"** matters. Relighting can otherwise re-render the text.

## Mockup formats worth having

| Format | Use |
|---|---|
| Single pack, front, white | Listing main image, approvals |
| Pack range, side by side | Line overview, category page |
| Pack in a scene | Lifestyle, social, ads |
| Pack held in hand | Scale, use. See `product-in-hand-shots` |
| Pack open / in use | Demonstration |
| Flat artwork dieline | Print approval — not a generated image |
| Secondary packaging | Shipping box, outer carton |

**A dieline is not a mockup.** Print approval needs the flat artwork at spec, not a render. Don't let a good mockup substitute for checking the file.

## Approvals and the honest caveat

Mockups get used for internal approval, and there's a real risk in that.

- **A mockup is not a proof.** Colour on screen isn't colour on press
- **Don't approve copy from a mockup.** Approve copy from the copy document
- **Don't approve colour from a render.** Approve from a printed proof or a Pantone reference
- **Mark mockups as mockups** when circulating them, so nobody signs off the wrong thing

A generated mockup with plausible-looking wrong text that gets approved and printed is a real and expensive failure mode.

## The QA pass

```
[ ] No generated text anywhere on the pack
[ ] Real artwork applied, at correct size and position
[ ] Artwork warped to the form, not pasted flat
[ ] Label finish matches the pack finish
[ ] Regulatory faces photographed, not generated
[ ] Pack proportions match the real pack
[ ] Colour verified against a physical reference, not a screen
[ ] Marked as a mockup if circulated for approval
```

## Don't

- **Don't generate packaging copy.** Ever.
- **Don't omit "no label, no text"** when generating a blank form.
- **Don't generate regulatory faces.** Photograph them.
- **Don't paste artwork flat** onto a curved form.
- **Don't approve copy or colour from a mockup.**
- **Don't use FLUX for any text**, even decorative. Use `gpt-image-2`.
- **Don't let relighting re-render the label text.**
- **Don't show the label larger than it is.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`image-model-selection`, `food-beverage-photography`, `marketplace-image-compliance`, `product-photo-from-reference`, `multi-product-scene`, `product-image-qa-review`
