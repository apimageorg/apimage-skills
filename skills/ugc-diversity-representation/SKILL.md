---
name: ugc-diversity-representation
description: Handle representation deliberately when generating creator personas, countering model defaults without producing tokenism or misrepresenting who a brand serves. Use whenever the user is generating people for creative, building a persona roster, or their generated presenters all look the same.
---

# Representation in Generated Creative

Image and video models have strong demographic defaults. Ask for "a person using a product" repeatedly and you get a narrow band back — typically young, light-skinned, slim, non-disabled, and conventionally attractive. That's a property of training data, not a choice you made, but it becomes your choice the moment you publish it.

Generation removes the usual excuse. Casting a varied roster costs a few credits.

## The default is narrow, so specify

The practical mechanic: **unspecified means default.** If you don't name age, body, skin tone or ability, the model picks, and its pick is not representative.

```
Weak:   "a woman using the product in a kitchen"
        → the same narrow output, every time

Specified: "a woman in her fifties, mid-size build, deep brown
            skin, short grey-flecked hair, ordinary appearance,
            plain jumper, no makeup"
```

Same principle as the "ordinary appearance" instruction generally: the model's prior pushes toward glossy and homogeneous, and countering it takes explicit words. See `ugc-creator-persona-design`.

## Represent the actual customer base

The commercial framing, and the honest one. This isn't decoration — it's accuracy about who buys.

- **Look at your real customer data.** Age distribution, geography, who actually purchases
- **Generate to that**, not to a demographic ideal
- **The mismatch costs money.** A product bought mostly by people over 50, advertised with 25-year-olds, converts worse — and that's testable
- **Recognition is the UGC mechanism.** A viewer who doesn't see themselves doesn't feel addressed

**Where your data and the model's default diverge is where the opportunity is**, because that's the segment your creative currently doesn't address and your competitors' probably doesn't either.

## Vary more than one axis

Tokenism is varying skin tone while holding everything else at the default — four people of different ethnicities, all 25, all slim, all in bright kitchens.

Axes worth varying across a roster:

```
Age               25 / 35 / 50 / 65
Body              a genuine range, not one non-slim person
Skin tone         a genuine range
Hair              texture and length, not one texture
Disability        visible aids: a stick, a chair, a hearing aid
Setting           a small rented flat, not four bright kitchens
Register          how they speak, not just how they look
Wardrobe          workwear, uniform, hijab, ordinary clothes
```

**Setting is the one people miss.** Four people of different backgrounds in four identical aspirational kitchens is a narrower roster than it looks. Class and circumstance are part of representation. See `ugc-lifestyle-photos`.

## Where the model needs help

Specific failure modes worth knowing before you spend on video.

| Under-served | What to do |
|---|---|
| Older faces | Ask for lines, grey hair, thinning hair explicitly. "In their sixties" alone often returns a youthful sixty |
| Darker skin tones | Specify lighting for the skin: "lit to hold detail in deep brown skin, no blown highlights" |
| Textured hair | Name the texture. Defaults smooth it |
| Larger bodies | Be specific. Euphemisms return slim |
| Disability | Name the aid and its correct use. Verify it's depicted correctly |
| Non-Western dress | Name the garment. Check it's worn correctly |
| Visible skin conditions | Ask explicitly. Defaults are flawless |

**Lighting for darker skin is a real technical issue, not a sensitivity point.** Default lighting setups are calibrated for light skin and produce crushed shadows or blown highlights on deep skin tones. "Lit to hold detail in deep brown skin, exposed for the skin not the background" measurably improves the output. See `product-relighting`.

**Disability depictions need verification by someone who knows.** A wheelchair at the wrong height, a stick in the wrong hand, a prosthetic attached implausibly — these read as careless to the people being depicted, which is worse than absence.

## Don't generate a demographic you can't stand behind

The boundary that matters, and it's easy to cross without noticing.

| Acceptable | Not |
|---|---|
| A varied roster reflecting your customers | Generating a community to claim endorsement from it |
| A disclosed presenter of any demographic | A generated person of a group testifying to experience |
| Representative casting | Ethnicity or disability as a marketing device |
| Consulting people about depictions of them | Guessing, and publishing |

Two specific traps:

**Fabricated community endorsement.** Generating a person of a particular group to say the product works for people like them is a false endorsement with an extra problem: you've used an identity you have no standing in to make the claim. The general rule that synthetic presenters can't claim experience applies with more force here, not less. See `ugc-disclosure-compliance`.

**Cultural specificity you haven't checked.** Religious dress, cultural settings, community contexts — generated approximations get details wrong in ways that are obvious to the people depicted and invisible to everyone else. Ask someone. It costs one conversation.

## Practical roster build

```
1. Pull your real customer demographics
2. Design 4-6 personas mapping to the actual distribution
3. Specify every axis explicitly. Nothing left to default
4. Generate 4-6 portraits each, fixed seed per persona
5. Review with someone from the group depicted, where relevant
6. Save as character brand assets                          free
7. Test the same script across the roster                  see below
```

Then run the variant test. **The performance data is the argument** that keeps representation in the plan rather than in the values statement — and in practice the older, less glossy personas frequently win. See `ugc-multi-creator-variants`.

## Don't

- **Don't leave demographics unspecified.** Unspecified means default.
- **Don't vary one axis** and call it a roster.
- **Don't put every persona in the same kind of home.**
- **Don't use default lighting** on deep skin tones.
- **Don't guess at disability or cultural detail.** Verify.
- **Don't generate a community to endorse you.**
- **Don't cast against your real customer base.**
- **Don't treat it as a values exercise.** It's testable, and it converts.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-creator-persona-design`, `ugc-multi-creator-variants`, `model-wearing-product`, `ugc-disclosure-compliance`, `product-relighting`, `ugc-legal-likeness-rights`
