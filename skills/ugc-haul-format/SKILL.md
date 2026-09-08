---
name: ugc-haul-format
description: Produce haul and multi-product video where several items are shown fast, with accurate prices and honest verdicts. Use whenever the user wants haul content, "what I bought" video, collection or bundle creative, or multi-SKU short-form.
---

# Haul Format

A haul shows several products quickly. It suits catalogue businesses, bundles, seasonal ranges and anything where the buying decision is *which one*, not *whether*.

It's cheap to generate because it's product-and-hands, no dialogue required. Its exposure is entirely in the details on screen: **every price, size and availability claim is an advertising claim**, and a haul contains a dozen of them.

## The structure

```
0-3s     THE FRAME        why these items, together. Specific
3-15s    THE ITEMS        6-10 of them, 1.5-2s each
15-17s   THE PICK         which one, and why
17-18s   ONE ACTION
```

**Six to ten items at 1.5-2 seconds each.** Slower and it drags; more items and none register. The pacing is the format.

The frame beat is what makes a haul content rather than a catalogue:

```
Weak:   "New arrivals haul"
Strong: "Everything under fifteen quid that actually works on
         oak, tested"
```

A constraint — a price ceiling, a use case, a room, a season — gives the viewer a reason to watch a list.

## Producing it

Fast, repeatable, no presenter needed.

```python
LOOK = ("Handheld phone footage, filmed from directly above at a "
        "slight angle, slight shake. Natural window light from "
        "the left, no fill. Ordinary wooden table, a phone and "
        "keys at the edge of frame. Unpolished, shot on a phone. "
        "Not studio, not commercial.")
SEED = 4271

def item(asset):
    return generate_video(
      mode="image-to-video",
      model="flux-3-video-draft",
      reference_images=[asset],
      prompt="A hand sets the product down on the table, turns it "
             "once to show the front, then lifts it out of frame. "
             "Simple single motion. " + LOOK,
      aspect_ratio="9:16", resolution="hd", duration=2, seed=SEED)
```

**Identical prompt, identical look, identical seed, one product asset swapped.** That's what makes ten clips feel like one haul filmed in one sitting — and it's a two-line loop. Batch them as drafts, enhance the set once. See `batch-video-production` and `product-photo-batch-pipeline`.

Video is always async: `generate_video` returns a job ID, and you poll `get_video_generation`. Fire all ten, then poll — don't wait on each in turn. Polling is free. See `async-video-job-orchestration`.

## Every product must be real

Ten products means ten chances to show something that isn't what ships.

```
[ ] Real photography as the reference for every item
[ ] Current packaging on all of them
[ ] Colours accurate
[ ] Nothing discontinued or out of stock
[ ] Nothing shown that isn't purchasable
[ ] No generated labels or printed text anywhere
```

**A haul is a shopping list.** Showing an item that can't be bought converts a piece of content into a customer-service problem, at volume. See `product-image-qa-review`.

## Prices on screen are the real risk

The thing that makes hauls specifically hazardous: a price is the most checkable claim in advertising, and a haul puts eight of them on screen.

- **Exact price, with currency.** Not "about a tenner"
- **Say the basis.** Whether it includes VAT, whether delivery is extra
- **Date the video** if prices move. "Prices as of [month]" on screen
- **Never show a sale price** without the qualifying period, and never leave a lapsed one running
- **Reference-price rules are strict.** "Was £40" needs the higher price to have been genuinely charged, recently, for a meaningful period. Many markets enforce this specifically
- **Set a review date and honour it.** A price that changed is a false claim from the day it changed

**Evergreen hauls are where brands accumulate false price claims silently.** The video keeps running; the price moved in March. Either date it on screen or diary the review.

## Give at least one honest verdict

The credibility mechanism. A haul where all ten items are great is a catalogue.

```
"This one I'd skip. It works, but the small size is better value
 and it's the same formula."

"This is the one I'd actually buy of the three."

"This didn't work on oak at all. Fine on pine."
```

**Ranking is more useful than praising.** "Which of these should I buy" is the question the viewer actually has, and answering it is both more persuasive and easier to substantiate than ten superlatives. See `ugc-review-format`.

## Scale, consistently

Ten products at ten apparent sizes is the most common haul failure, because each clip is generated independently.

- **Same camera distance and angle** in every clip. The fixed prompt handles this
- **A constant reference object in frame** — the same hand, the same mug at the edge — gives the eye a scale anchor across cuts
- **State dimensions on screen** for anything where size matters
- **Check the set played in sequence**, not clip by clip

See `product-scale-reference`.

## Disclosure

- **Own products on a brand account:** it's an ad. Label it plainly on screen
- **Gifted or affiliate items:** disclose in-video, not only in the caption. This is heavily enforced
- **Mixed hauls** with own and third-party products need the commercial relationship clear per item
- **Generated footage** needs the platform's AI-content label as well. Separate obligation
- **Don't generate third-party packaging.** Trade mark, and inaccuracy

See `ugc-disclosure-compliance` and `ugc-affiliate-creative`.

## Don't

- **Don't show more than about ten items.**
- **Don't linger.** 1.5-2 seconds each.
- **Don't open without a constraint** or frame.
- **Don't vary the framing** between items.
- **Don't approximate prices.** Exact, with basis.
- **Don't leave a lapsed sale price** running.
- **Don't praise everything.** Rank them.
- **Don't show anything unavailable** or discontinued.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`batch-video-production`, `multi-product-scene`, `product-scale-reference`, `ugc-unboxing-video`, `ugc-affiliate-creative`, `ugc-tiktok-shop-content`
