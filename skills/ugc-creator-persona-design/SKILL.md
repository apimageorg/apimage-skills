---
name: ugc-creator-persona-design
description: Design a synthetic creator persona that suits the product and audience, and build the reference set that keeps them consistent. Use whenever the user needs a UGC creator, a recurring on-camera personality, a brand spokesperson character, or several creators for variant testing.
---

# Designing a Creator Persona

A generic "person" produces generic content. The persona decides whether the audience recognises themselves — which is the entire mechanism by which UGC works.

Design them before generating, and save them so the whole campaign uses the same person.

## Design from the audience, not from aesthetics

The instinct is to generate an attractive presenter. That's usually wrong: UGC works because the viewer thinks *this is someone like me*, and an obviously professional-looking presenter breaks that.

```
Wrong:  "attractive young woman, perfect skin, professional"
Right:  "woman in her late thirties, ordinary appearance, tired but
         friendly, plain grey t-shirt, no makeup"
```

**"Ordinary appearance" is a deliberate and effective instruction.** The model's prior pushes toward glossy; countering it produces someone who reads as a real person.

## The persona brief

Decide these before generating. Write them down.

```
Age band          late thirties
Presentation      ordinary, unpolished, approachable
Wardrobe          plain grey t-shirt, no jewellery
Setting           an ordinary kitchen, some clutter
Energy            calm and direct, slightly tired
Speech register   plain, conversational, uses contractions
Relationship to
the product       a spokesperson explaining it — NOT a customer
```

That last line is the one that matters legally, and it should be decided at design time rather than drifting into testimony later. See `ugc-disclosure-compliance`.

## Match the persona to the product

| Product | Persona that works |
|---|---|
| Household, cleaning | Someone who clearly does the cleaning. Practical, unfussy |
| Skincare, beauty | Age-appropriate to the claim. Visible real skin |
| B2B software | Looks like a colleague. Office-casual, slightly harried |
| Fitness | Realistically fit, not a model |
| Parenting | Visibly a parent. Tired, warm, in a real home |
| Trade tools | Working hands, workwear, a real site or workshop |
| Food | Someone who actually cooks. Kitchen with use marks |

**The mismatch is the common failure.** A polished 25-year-old presenting a mortgage product, or a model-looking presenter demonstrating a mop, breaks the recognition the format depends on.

**Age-appropriate matters for claims too.** A skincare product addressing lines on a presenter with no lines is both unpersuasive and arguably a misleading depiction.

## Generate the face

```
generate_image(
  model="flux-2-pro",
  prompt="Portrait of a woman in her late thirties, ordinary "
         "appearance, shoulder-length brown hair slightly untidy, "
         "plain grey t-shirt, no makeup, neutral friendly "
         "expression, faint lines around the eyes. Front-facing, "
         "both eyes visible, mouth closed. Soft even natural light "
         "from a window, no hard shadow on the face. Plain "
         "warm-white background. Sharp focus on the eyes. Not a "
         "model, not glamorous, not studio.",
  aspect_ratio="1:1",
  seed=8812
)
```

Two things:

**The negative framing** — "not a model, not glamorous, not studio" — is doing as much work as the positive description.

**The technical requirements** (front-facing, mouth closed, no hard shadow, sharp on the eyes) exist because a lip-sync render inherits every problem in the source. A shadow across the mouth produces mushy mouth shapes at 3 credits per second. See `lip-sync-spokesperson-video`.

Iterate freely here — image generation is 1-9 credits and this is the cheap half of the job.

## Build the reference set

One portrait gives the model one view. Three or four give it a face it can hold through motion.

```
seed=8812  "...front-facing..."                    → base
seed=8812  "...turned slightly left, 20 degrees..."
seed=8812  "...turned slightly right, 20 degrees..."
seed=8812  "...slight smile, front-facing..."
```

Hold the seed, change only the angle or expression.

```
create_brand_asset(type="character", ...)     # free
```

Also save the setting and the look:

```
create_brand_asset(type="background", ...)    # their kitchen
create_brand_asset(type="preset", ...)        # the handheld, natural-light look
```

All free. Now every clip in the campaign starts from a fixed person in a fixed place with a fixed look. See `ugc-character-consistency`.

## Build a small roster, not one creator

The strongest reason to generate rather than commission: several creators for the price of a few credits each.

```
Creator A   late thirties, practical, kitchen
Creator B   mid twenties, energetic, small flat
Creator C   fifties, calm, garden or workshop
Creator D   early thirties, office setting
```

Then test the same script across all four. **Which creator resonates is not predictable** — including by people who are good at this — and it's usually a larger performance variable than the script.

Each is one afternoon of image iteration plus a free brand asset. See `ugc-multi-creator-variants` and `ugc-diversity-representation`.

## Voice as part of the persona

The persona includes how they speak, and it should be consistent.

```
Creator A:  plain, practical, short sentences, mild understatement
Creator B:  fast, enthusiastic, informal, sentence fragments
Creator C:  measured, warm, longer sentences
```

**Write the script in their register**, not in brand voice. A synthetic creator delivering marketing copy is the most obvious failure in the whole format — the visual says "real person" and the words say "advertising department". See `ugc-script-writing`.

## The identity decision, made explicitly

Decide at design time what this person *is*, because it governs what they can say.

| Identity | Can say | Cannot say |
|---|---|---|
| **Disclosed brand spokesperson** | How it works, what it does, substantiated claims | "I bought this" |
| **Disclosed AI presenter** | Same | Personal experience |
| Real customer | Their genuine experience, with permission | — |
| **Undisclosed "customer"** | — | **Nothing. Don't build this** |

The workable persona is a **disclosed presenter or spokesperson**. That keeps the format's visual advantage without fabricating testimony, and it's a decision that's much easier to make now than to retrofit after a campaign is written.

## Don't

- **Don't generate an attractive presenter by default.** Ordinary works better.
- **Don't omit the negative framing.** "Not a model, not studio."
- **Don't mismatch persona to product.**
- **Don't build from one reference image.**
- **Don't skip the brand assets.** They're free and they're the persona.
- **Don't write in brand voice.** Write in the persona's register.
- **Don't leave the identity undecided.** Spokesperson, not customer.
- **Don't build a persona whose premise is being a real customer.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-character-consistency`, `ai-avatar-presenter`, `ugc-script-writing`, `ugc-multi-creator-variants`, `ugc-diversity-representation`, `ugc-disclosure-compliance`
