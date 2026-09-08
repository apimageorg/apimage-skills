---
name: ugc-lifestyle-photos
description: Generate lifestyle stills that place a product in a believable real environment rather than a styled set. Use whenever the user needs in-context product images, room or setting shots, environmental product photography, or images that show where and how a product is used.
---

# Lifestyle Stills, The Real-Room Version

Lifestyle photography answers a question the product shot can't: *where does this live?* For furniture, homeware, tools and anything spatial, that's the actual purchase blocker.

There's a polished version of this — the styled interiors shot — and a UGC version, which is the same room without the stylist. This skill is the second one. It's harder to prompt, because "an ordinary room" is not what a model reaches for.

## The difference from a styled set

| Styled interior | Real room |
|---|---|
| Everything placed | Things where they were left |
| Coordinated palette | Mismatched, accumulated |
| Nothing worn | Visible wear, marks, use |
| Clear surfaces | Some clutter |
| Even, designed light | One window, uneven |
| No cables, no post, no mugs | All of those |
| Product is the focal point | Product is *in* the room |

**Both are legitimate. They do different jobs.** The styled version sells aspiration; the real-room version sells plausibility. Pinterest wants the first; cold social ads usually want the second. See `ugc-vs-polished-decision`.

## Prompting an ordinary room

The clauses that produce a lived-in space:

```
"Ordinary rented flat, not designed"
"Mismatched furniture, accumulated over time"
"Visible wear on the surfaces, marks and scuffs"
"Some clutter — post on the side, a mug, a cable"
"Single window light from the left, no fill, uneven across the room"
"Deep focus, the whole room in focus"
"Nothing arranged or styled"
"Not an interiors magazine, not a show home, not styled"
```

A working example:

```
generate_image(
  model="flux-2-pro",
  prompt="An amber glass bottle standing on a wooden kitchen "
         "worktop in an ordinary rented flat. Mismatched cupboard "
         "fronts, visible wear and water marks on the worktop, a "
         "kettle, an open packet, some post and a mug left where "
         "they were put. Single window light from the left, "
         "uneven across the room, no fill. Shot on a phone at "
         "standing height, slight wide-angle distortion, deep "
         "focus with the whole room sharp. Not an interiors "
         "magazine, not a show home, not styled, no softbox.",
  reference_images=[PRODUCT_ASSET],
  aspect_ratio="4:5",
  seed=8812
)
```

**"Accumulated over time" and "where they were put"** are the two phrases that most reliably break the model's styling instinct. Naming specific ordinary objects — post, a mug, a cable — works better than asking for clutter in the abstract.

## Match the room to the buyer, not to a magazine

The commonest failure is generating an aspirational space that the target customer doesn't live in.

| Product | Room that sells it |
|---|---|
| Budget homeware | Rented flat, small kitchen, mismatched |
| Family products | Visible children's things, mess, warmth |
| Premium furniture | Considered but lived-in. Not a showroom |
| Tools | A real garage or site. Dust, other tools |
| Office products | An actual desk. Cables, a second monitor, a mug |
| Fitness | A corner of a bedroom, not a gym |
| Kitchen consumables | A worktop mid-use, not cleared |

**The mismatch reads as an ad instantly.** A £15 cleaning product in an architectural kitchen tells the viewer this isn't for them.

## Keep the room consistent across a set

A lifestyle set should look like one home photographed several times.

```
create_brand_asset(type="background", ...)   # the kitchen. Free
create_brand_asset(type="preset", ...)       # the phone-photo look. Free
```

Then hold the seed and vary only the angle or the product placement. Same room, several photos — which is what a real customer's photo set looks like.

`replace_background` (3 credits) is the alternative route: shoot the product once cleanly and place it into a generated room. Often better than generating product-in-room from scratch, because the product stays exactly accurate. See `background-replacement-scenes` and `furniture-room-scenes`.

## Scale is the point, so get it right

For anything spatial, the lifestyle shot's real job is conveying size — and a wrong size is a returns problem.

- **Include a known object.** A mug, a plug socket, a door frame, a hand
- **Standing eye height** for room shots. A low angle inflates apparent size
- **Check against the real dimensions** before publishing. Measure it in the frame against the reference object
- **State dimensions on screen or in the caption** for furniture and large items

**Generated room shots misjudge scale often**, and it's the one error customers notice on delivery. See `product-scale-reference`.

## Light describes the room, and the time

The light does more than expose the shot — it says what kind of place this is and when.

```
Early morning     "cold blue light, low, one window, most of the
                   room still dark"
Midday            "flat bright daylight, slightly blown out near
                   the window"
Late afternoon    "low warm sun raking across the floor, long
                   shadows"
Evening           "warm lamp light only, uneven, dark corners"
Overcast          "soft grey even light, low contrast, no shadows"
```

**"Uneven" and "dark corners" are the realism clauses.** Real rooms have badly-lit parts. An evenly-lit room is a set.

See `product-relighting`.

## The accuracy floor

Lifestyle framing tempts you to be loose about the product. Don't be.

```
[ ] Real product photography as the reference
[ ] Product proportions correct against the room
[ ] Label and logo undistorted where visible
[ ] No generated printed text
[ ] Colour accurate under the stated light
[ ] Nothing shown that isn't included
[ ] Scale checked against a known object
```

Run the full QA pass. The relaxed aesthetic makes real defects easier to miss. See `product-image-qa-review`.

## Don't

- **Don't generate a styled interior** when you asked for a real room.
- **Don't omit the negative clauses.** "Not a show home."
- **Don't ask for clutter abstractly.** Name the objects.
- **Don't mismatch the room to the buyer.**
- **Don't light the room evenly.**
- **Don't shoot rooms from a low angle.** It inflates scale.
- **Don't publish without a scale check.**
- **Don't let the setting excuse product inaccuracy.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lifestyle-product-photography`, `furniture-room-scenes`, `background-replacement-scenes`, `product-scale-reference`, `ugc-selfie-style-photos`, `product-relighting`
