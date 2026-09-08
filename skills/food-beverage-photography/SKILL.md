---
name: food-beverage-photography
description: Generate food and drink product imagery, including the regulatory line on depicting food accurately. Use whenever the user is photographing food, drink, packaged groceries, restaurant dishes, supplements or anything edible.
---

# Food and Beverage Photography

Food generates well — texture, steam, condensation and freshness are exactly what image models are good at. That's also the problem: they generate an *idealised* version of the food, and food advertising is regulated on accuracy in most markets.

The craft is making it appetising while it still represents what arrives.

## The regulatory line

Food imagery rules vary by market and they're enforced. The consistent principles:

**Not acceptable:**
- Depicting more product than the pack contains
- Showing ingredients not in the product
- Implying freshness, size or quality the product doesn't have
- Serving suggestions not marked as such
- Health or nutrition claims implied visually that aren't substantiated
- A prepared dish that can't be made from the pack contents as sold

**Acceptable:**
- A serving suggestion, **clearly labelled as one**
- Styling that presents the actual product well
- Steam, condensation, garnish — where honest about what's included
- Normal food styling practice, disclosed where required

**The test: could the customer produce this from what's in the pack?** If the photo shows the cereal with fresh berries that aren't included, that's a serving suggestion and it must say so.

Supplements and anything with a health claim sit under stricter rules again — those go through legal review before publishing. See `product-image-qa-review`.

## Packaged food: photograph the pack

For packaged grocery products the pack itself is the product, and packs carry legally required text.

**Photograph the pack. Never generate it.**

- Ingredients lists, allergen declarations, nutrition panels and net weight are regulatory copy
- Models render text badly, and invented ingredient text is a serious compliance failure — not a cosmetic one
- Even where the front-of-pack is generated convincingly, the copy will be wrong

Generate the *scene* around a photographed pack. `replace_background` at 3 credits gives you kitchen, table, picnic and shelf contexts with the pack intact. See `background-replacement-scenes`.

## Prepared food and dishes

Where generation is genuinely useful — restaurant menus, recipe content, serving suggestions.

```
generate_image(
  model="flux-2-pro",
  reference_images=[DISH_PHOTO],
  prompt="The same dish on a matte ceramic plate, three-quarter view "
         "from slightly above. Visible steam rising gently. Soft "
         "directional window light from the left with warm falloff. "
         "Shallow depth of field, blurred timber table behind. "
         "Natural, appetising, not over-styled. Dish unchanged — "
         "same components and portion.",
  aspect_ratio="4:3",
  seed=8812
)
```

The vocabulary that makes food read as appetising:

| Element | Prompt language |
|---|---|
| Freshness | "visible moisture", "just-cut edges", "crisp" |
| Temperature | "gentle steam rising", "condensation on the glass" |
| Texture | "visible crumb", "caramelised edges", "glossy glaze" |
| Light | "soft directional window light", "warm falloff" |
| Depth | "shallow depth of field", "blurred background" |
| Angle | "three-quarter from slightly above" — the food standard |
| Restraint | "natural, not over-styled" |

**Three-quarter from slightly above is the food angle.** It shows the surface and the depth simultaneously. Straight-down works for flat lay; straight-on rarely works for a plated dish.

**"Not over-styled" is worth including.** The model's default for food is glossy, symmetrical and slightly plastic. Prompting for natural imperfection — an uneven edge, a crumb, a drip — is what makes it look edible rather than rendered.

## Drinks

Their own set of problems, mostly reflective.

- **Glass and bottles** are transparent and reflective. See `jewelry-reflective-products` — the same halo and reflection issues apply
- **Condensation** generates well and signals cold effectively
- **Carbonation** — bubbles rising — is a strong cue and generates reasonably
- **Pour shots** are motion, so they belong in video rather than a still. See `product-demo-video`
- **Liquid colour** must be accurate. A juice rendered more saturated than it is misrepresents the product
- **Ice** generates inconsistently. Photograph it where it matters

**Never generate a liquid's colour or clarity.** Reference-based only, verified against the real product.

## What generates badly

| Subject | Problem |
|---|---|
| Text on packaging | Garbled. Photograph it |
| Hands handling food | Fingers, joints, plausibility |
| Cheese pulls, stretch shots | Physics goes wrong |
| Many small identical items | Count drifts, shapes vary |
| Cut fruit interiors | Seed and segment structure invented |
| Branded packaging in a scene | Logos re-drawn |
| Precise portion sizes | Drifts, which is a claim problem |

**Portion accuracy is the one with commercial consequence.** A generated bowl containing visibly more than the pack provides is exactly what the regulations address.

## The photography-plus-generation split

The workflow that keeps you compliant and still saves the shoot:

```
Photograph:   the pack, the actual dish, the actual portion,
              anything with text, anything with a regulatory claim
Generate:     the surface, the background, the light, the setting,
              the props, seasonal variations, the scene
```

One photograph of the real dish plus `replace_background` gives you a summer picnic, a winter table, a restaurant setting and a kitchen counter — for 3 credits each, with the food unchanged.

See `product-photo-from-reference` and `seasonal-product-styling`.

## The QA pass

```
[ ] Portion matches what the pack provides
[ ] No ingredients shown that aren't included
[ ] Serving suggestion labelled if it is one
[ ] Pack text photographed, legible and correct
[ ] Liquid colour and clarity accurate
[ ] Item counts correct
[ ] No implied health or nutrition claim
[ ] Allergen and nutrition panels unaltered
[ ] Colour verified against the real food
```

## Don't

- **Don't generate packaging text.** Photograph it. Compliance, not aesthetics.
- **Don't show more product than the pack contains.**
- **Don't add ingredients that aren't included** without labelling a serving suggestion.
- **Don't generate liquid colour or clarity.**
- **Don't generate cut-fruit interiors.**
- **Don't let the model over-style it.** Prompt for natural.
- **Don't generate hands handling food.**
- **Don't publish supplement or health-claim imagery** without legal review.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-replacement-scenes`, `flat-lay-composition`, `jewelry-reflective-products`, `seasonal-product-styling`, `product-image-qa-review`, `packaging-mockup-generation`
