---
name: ugc-review-format
description: Produce honest-review-format video — the comparison, the criteria, the caveat — without fabricating a reviewer's experience. Use whenever the user wants review-style creative, "I tried X" content, product comparison video, or verdict-driven short-form.
---

# Review Format

Review content converts because it sounds like someone deciding rather than someone selling. Which means the format's whole value rests on the reviewer being independent, and a brand generating its own reviewer is the exact thing the format is trusted for not being.

There's still a real, high-performing version available. It's the **evaluation**, not the verdict.

## What breaks and what doesn't

| Framing | Legitimate |
|---|---|
| "Here's how to choose between options in this category" | **Yes** |
| "This is designed for X, not Y, and here's how to tell" | **Yes** |
| Brand comparing itself to named competitors on facts | **Yes**, if accurate and substantiated |
| A real reviewer's words, generated b-roll | **Yes**, with permission |
| Generated person: "I tried three and this won" | **No** |
| Generated person posing as an independent reviewer | **No** |
| Anything implying an unpaid opinion | **No** |

**The distinction:** a buying guide teaches criteria. A review reports an experience. You can generate the first honestly; the second needs a person who actually had it.

Reframing from verdict to criteria keeps almost all the performance. Viewers watch review content to learn *how to decide*, and the verdict is what they'd rather reach themselves.

## The buying-guide structure

```
0-3s     THE CONFUSION      the real decision people get stuck on
3-7s     CRITERION ONE      the thing that actually matters
7-11s    CRITERION TWO      the thing everyone checks that doesn't
11-15s   THE FIT            who each option suits
15-18s   WHERE OURS SITS    plainly, disclosed
```

```
"Every worktop oil claims to restore. The question is whether
 your finish is oil or lacquer, and most people don't know.
 Here's the test. And if it's lacquer, don't buy any of them,
 including ours."
```

That last clause is worth more than everything before it.

## Disqualifying yourself is the technique

The one move that makes brand-authored review content credible: say who shouldn't buy it.

```
"If your worktop is lacquered, this won't work. Ours included."

"Under about ten square metres, the small bottle is fine.
 Don't buy the big one."

"If you want a one-coat finish, this isn't it. It's two."
```

Three effects, all commercial:

- **Credibility transfers** to every other claim in the video
- **Returns drop**, because the wrong buyer self-selects out
- **It's substantiated by construction.** A limitation is a fact about your own product

Brands resist this hardest and it's the highest-yield thing in the format. See `ugc-script-writing`.

## Comparison claims: the rules

Naming competitors is legal in most markets and heavily conditioned. If you do it:

- **Compare like with like.** Same size, same spec, same price basis
- **Be current.** A price that changed last month is a false claim now
- **Be specific.** "Better" is unsubstantiated; "covers 12 square metres versus 8" is a fact
- **Keep the substantiation** dated, sourced and on file before publishing
- **Don't denigrate.** Factual comparison is fine; disparagement invites a complaint
- **Never generate a competitor's packaging or logo.** Trade mark and misrepresentation both

**Set a review date** on any comparison creative. Comparison claims decay silently: the ad keeps running after the fact stops being true, and that's the most common way brands end up with a false claim they never intended to make.

## Producing it

No presenter needed for most of it, which makes it cheap.

```python
LOOK = ("Handheld phone footage, filmed from above, slight shake. "
        "Natural window light, ordinary kitchen table. Unpolished, "
        "shot on a phone. Not studio, not commercial.")

BEATS = [
  ("Two bottles side by side on the table, a hand turns one to "
   "show the back label, then the other.", 5),

  ("A hand runs a fingertip across two patches of the same "
   "worktop, one treated and one not. Camera locked.", 5),
]
```

Own product from real photography, on-screen text for the criteria, no fabricated reviewer. If you show two products side by side, **only yours may be a real reference**, per the trade mark point above.

## Using real reviews in this format

If you have genuine reviews, the strongest review-format creative is real words over generated scenery.

```
0-3s     A real quote on screen, attributed, including its caveat
3-10s    Generated b-roll of the product in use
10-15s   A second real quote, ideally a critical one you answered
15-18s   CTA
```

**A real three-star review you address honestly outperforms a wall of five stars.** Keep the verbatim, the date and the written permission on file. See `ugc-testimonial-video`.

## Disclosure

Review-format creative from a brand needs disclosure to be unmissable, because the format's entire premise is independence.

- **On screen, in the first three seconds.** Not in the caption
- **Plain words.** "From [Brand]" or "Ad", not a hashtag at the end of a caption
- **Plus** the platform's AI-content label if the footage is generated. Two separate obligations
- Even where "it's obviously a brand account" feels true, it isn't a defence

See `ugc-disclosure-compliance`.

## Don't

- **Don't generate an independent reviewer.** Not a soft line.
- **Don't state a verdict** you have no basis for. Teach criteria.
- **Don't skip the disqualifier.** It's the whole credibility mechanism.
- **Don't make unsubstantiated comparisons.** File the evidence.
- **Don't generate competitor packaging or logos.**
- **Don't let comparison claims run past their review date.**
- **Don't bury disclosure in a caption.**
- **Don't hide critical reviews.** Answer one on screen.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-testimonial-video`, `ugc-disclosure-compliance`, `ugc-script-writing`, `ugc-problem-solution-format`, `video-brand-safety-moderation`, `ugc-comment-reaction-format`
