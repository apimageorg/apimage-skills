---
name: seasonal-product-styling
description: Produce seasonal and campaign variants of product imagery from one master shot, using background replacement rather than reshoots. Use whenever the user needs Christmas or summer or back-to-school imagery, campaign variants, or seasonal refreshes of a catalogue.
---

# Seasonal and Campaign Variants

Seasonal imagery is the clearest commercial case for generated product photography. A retailer needs the same product in autumn, Christmas, spring and summer contexts — four shoots, or one shoot and `replace_background` at 3 credits a scene.

The product stays identical across all of them, which is both the point and the accuracy requirement.

## The workflow

```
1. One accurate master product shot          photograph or generate
2. remove_background            2 credits    → clean cutout, saved
3. create_brand_asset                        → free, reusable forever
4. replace_background           3 credits    → each seasonal scene
5. Verify colour and shadow per variant
```

**5 credits gets you the cutout. Every subsequent season is 3.** That's the economics that makes this worth doing rather than reshooting.

See `background-removal-workflow` and `background-replacement-scenes`.

## Seasonal scene language

The prompts that actually read as a season, rather than as a product with a coloured background.

```python
SEASONS = {
  "spring":    ("Pale birch surface, soft bright morning light through "
                "a window, a few white blossom sprigs slightly out of "
                "focus, fresh cool-neutral grade, light and airy."),
  "summer":    ("Weathered timber table outdoors, dappled hard sunlight "
                "through leaves, strong short shadows, a linen cloth, "
                "warm saturated grade."),
  "autumn":    ("Dark oak surface, low warm afternoon light raking from "
                "the left, long soft shadows, a few dried leaves and "
                "a woollen texture, amber grade."),
  "christmas": ("Dark matte surface, warm low lamplight from the right, "
                "soft bokeh of small warm lights in the background, "
                "a sprig of fir, deep warm grade."),
  "new_year":  ("Polished dark surface with reflection, cool directional "
                "light, minimal styling, high contrast, crisp grade."),
  "back_to_school": ("Light wood desk, bright even daylight, a notebook "
                "and pencil slightly out of focus, neutral clean grade."),
}
```

**Light is what says season, more than props.** Low raking warm light says autumn regardless of what's on the table. Hard dappled sunlight says summer. Soft bright cool light says spring. Getting the light right does more than adding a leaf.

Props are the secondary cue and they should be minimal — one or two, out of focus. Three seasonal props reads as a stock photo.

## The accuracy requirement

Seasonal grading is where product colour goes wrong, and it's the most common commercial failure of this workflow.

- **A Christmas warm grade shifts every colour.** Navy reads black, white reads cream, grey reads brown
- **Keep a neutral master for the listing image.** Seasonal variants are for ads, social, email and secondary slots — never the main listing image
- **Verify colour per variant** against the physical product
- **Never let a seasonal grade become the product's apparent colour**

The rule: **seasonal variants are marketing images; the listing image stays neutral.** See `white-background-ecommerce` and `product-image-qa-review`.

## Campaign consistency

A seasonal campaign runs across placements, and it should read as one campaign.

```
Fixed across the campaign:
  the product cutout (brand asset)
  the seasonal scene (saved as a background brand asset)
  the grade, stated verbatim
  the seed

Varies:
  aspect ratio per placement — generated native, not cropped
  the product, across the range
```

```
create_brand_asset(type="background", ...)   # the season, saved
create_brand_asset(type="preset", ...)       # the grade, saved
```

Save the season as a background asset the first time you get it right. Next year it's already there, and every product in the range gets the identical treatment. See `product-photo-consistency`.

## Generate each ratio natively

A campaign needs several ratios, and cropping one master loses the composition.

| Placement | Ratio |
|---|---|
| Email header | 16:9 |
| Social feed | 1:1 or 3:4 |
| Story / Reels | 9:16 |
| Web hero | 16:9 or 21:9 |
| Paid social | 1:1 and 4:5 (use 3:4) |
| Listing secondary | 1:1 |

Same cutout, same scene prompt, same seed, different `aspect_ratio`. The scene recomposes for each frame rather than being cropped out of one. See `aspect-ratio-strategy`.

## Plan ahead of the season

The practical constraint that catches retailers.

- **Christmas assets are needed in September**, not December
- **Summer assets in March**
- **Back-to-school in June**
- **Build next season's assets while the current one runs**

Because the workflow is 3 credits a scene rather than a shoot, working ahead is genuinely cheap. The bottleneck is deciding, not producing.

## Regional and cultural variation

An international catalogue can't use one seasonal set.

- **Seasons invert** in the southern hemisphere. A "summer" scene in December is correct for Australia and wrong for Europe
- **Holidays differ.** Christmas imagery is inappropriate in many markets, and other festivals matter more
- **Cultural signifiers vary** — colours, symbols and motifs carry different meanings
- **Weather cues** should match the market's actual climate

Generate per market rather than translating one set. It's 3 credits a scene, so there's no cost argument for a single global set that fits nobody.

## The QA pass

```
[ ] Product colour verified against the physical item, per variant
[ ] Product shape and label unchanged across all variants
[ ] Shadow direction matches the scene's stated light
[ ] Shadow length and softness match the light quality
[ ] Season reads from the light, not just the props
[ ] Props minimal and not implying included items
[ ] Neutral master retained for the listing image
[ ] Each ratio generated natively, not cropped
[ ] Regional appropriateness checked per market
```

## Don't

- **Don't use a seasonal variant as the main listing image.**
- **Don't let the grade change the product's apparent colour.**
- **Don't rely on props for the season.** Light does the work.
- **Don't use more than two seasonal props.**
- **Don't crop one master into every ratio.** Generate native.
- **Don't use one seasonal set globally.** Seasons invert, holidays differ.
- **Don't rebuild the season next year.** Save it as a brand asset.
- **Don't leave it until the season starts.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`background-replacement-scenes`, `lifestyle-product-photography`, `product-photo-consistency`, `flat-lay-composition`, `social-commerce-product-images`, `product-image-qa-review`
