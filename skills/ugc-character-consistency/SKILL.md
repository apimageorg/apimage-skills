---
name: ugc-character-consistency
description: Keep a UGC creator, their setting and their look identical across a whole campaign using brand assets. Use whenever a creator's face changes between clips, a campaign needs a recurring presenter, or the user is producing many UGC clips that should feel like one person.
---

# Keeping a Creator Consistent

A UGC campaign where the "creator" looks slightly different in every clip doesn't have a creator. It has a series of strangers, and the recognition that makes the format work never accumulates.

Consistency is free on APImage — the brand asset operations cost nothing. The only requirement is saving things at the moment you approve them.

## The four assets to save

```
create_brand_asset(type="character", ...)     the creator. Free
create_brand_asset(type="product", ...)       the product. Free
create_brand_asset(type="background", ...)    their kitchen. Free
create_brand_asset(type="preset", ...)        the handheld look. Free
```

All four, every time. Then a new clip is a new prompt against a fixed visual world rather than a fresh roll of the dice.

```
list_brand_assets(type="character")           free
get_brand_asset(type="character", id="...")   free
generate_brand_asset(...)                     generate AND save in one call
```

## Seeds give a look. Assets give a person.

The distinction people get wrong, and it's the reason "I locked the seed and the face still changed."

```
seed=8812  "a woman in her thirties, kitchen"    → person A
seed=8812  "a woman in her thirties, outdoors"   → person B, similar vibe
```

A locked seed produces a consistent **visual treatment** — grade, light quality, rendering character. It does not produce a consistent **subject**.

You want both: references for identity, seed for treatment. See `seed-locked-iteration`.

## Build a reference set, not one portrait

One front-facing image gives the model one view to work from. Three or four give it a face it can hold through motion.

```
seed=8812  "...front-facing, neutral expression..."     → base
seed=8812  "...turned slightly left, 20 degrees..."
seed=8812  "...turned slightly right, 20 degrees..."
seed=8812  "...slight smile, front-facing..."
```

Hold the seed; change only the angle or expression. That keeps it recognisably one person while giving the model coverage.

**Video models accept 9-30 reference images** depending on the model. Use more of them than you think you need — identity drift is the most common UGC failure and references are the fix.

## Never describe the creator in the prompt

The rule that fixes most drift.

**Wrong — competes with the references:**
```
A woman in her thirties with brown hair wearing a grey t-shirt holds
the bottle up to camera in a kitchen
```

**Right — describes only what's happening:**
```
She holds the bottle up toward the camera, already mid-sentence,
natural small head movements. Camera locked, handheld feel.
Everything else stays still.
```

The references carry identity. Re-describing appearance gives the model a second, competing specification and it splits the difference — which is exactly the drift you're trying to avoid. Same principle as `image-to-video-animation`.

## The whole-campaign spec

```python
CREATOR = ["<char ref 1>", "<char ref 2>", "<char ref 3>"]
PRODUCT = "<product asset>"
BG      = "<background asset>"
LOOK    = ("Handheld phone footage, slight shake, slightly uneven "
           "exposure. Natural window light from the left, no fill. "
           "Ordinary kitchen with some clutter visible. Unpolished, "
           "shot on a phone. Not studio, not commercial.")
SEED    = 4271

def clip(action, dur):
    return generate_video(
        mode="image-to-video",
        model="flux-3-video-draft",
        reference_images=CREATOR + [PRODUCT, BG],
        prompt=f"{action} {LOOK} Everything else stays still.",
        aspect_ratio="9:16",
        resolution="hd",
        duration=dur,
        seed=SEED,
    )
```

**Copy-paste the `LOOK` string. Never paraphrase it.** "Handheld with slight shake" and "slightly shaky handheld" are the same sentence to a human and different prompts to a model — and across twenty clips the paraphrases compound into twenty slightly different worlds.

## Where consistency breaks anyway

Be realistic about the limits, so you design around them.

| Cause | Mitigation |
|---|---|
| Hard profile angles | Keep to front and three-quarter |
| Long clips | Shorter beats, assembled |
| Fast motion | Slower motion, or accept softer identity |
| Prompt describing appearance | Remove it |
| Too few references | Use more |
| Different model between clips | Fix the model for the campaign |
| Hands near the face | Avoid the combination |

**Identity holds better over 5-8 seconds than over 20.** The practical answer to most drift is shorter clips cut together — which also cuts better and costs less. See `multi-scene-video-assembly`.

## Consistency for the product too

The creator isn't the only thing that has to stay the same. Product accuracy is the commercial requirement.

- **Real product photography** as the reference, never a generated approximation
- **Several angles** saved as one product asset
- **Modest motion.** Labels and logos distort where motion is greatest
- **Check the high-motion frames**, not a calm still

See `product-demo-video` and `product-image-qa-review`.

## Auditing a campaign for drift

Before publishing a set:

```
[ ] Play every clip in sequence. Is it one person?
[ ] Freeze a frame from each and compare side by side
[ ] Check the face at the highest-motion moment of each clip
[ ] Same setting throughout, or deliberately different?
[ ] Same grade and light quality across clips
[ ] Product identical in every clip — label, shape, colour
[ ] Wardrobe consistent, or deliberately changed?
```

**Side-by-side stills is the check that catches it.** Drift is invisible clip-by-clip and obvious in a row of frozen frames.

Wardrobe is worth a specific mention: unless it's a deliberate multi-day format, the creator wearing a different top in every clip undermines the single-person impression. State the wardrobe in the look clause.

## Don't

- **Don't rely on seeds for identity.** They give treatment, not subject.
- **Don't build from one reference image.**
- **Don't describe the creator's appearance** in a prompt that has references.
- **Don't paraphrase the look clause.** Copy it verbatim.
- **Don't skip the brand assets.** They're free.
- **Don't push identity through 20 seconds** of fast motion.
- **Don't change models mid-campaign.**
- **Don't publish a set without the side-by-side stills check.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`character-consistency-video`, `ugc-creator-persona-design`, `ai-avatar-presenter`, `seed-locked-iteration`, `multi-scene-video-assembly`, `ugc-multi-creator-variants`
