---
name: talking-head-scripting
description: Write scripts for spokesperson and avatar video that fit the duration, sound like speech, and survive lip-sync generation. Use whenever the user is writing a script for a talking-head video, an avatar presenter, a voiceover, or has a script that runs too long or sounds written.
---

# Scripting for Talking-Head Video

Two constraints govern this, and both are unusual.

**Cost is per second.** `generate_lip_sync` bills at 3 credits/second with a 30-second cap. A 30-second clip is 90 credits — a third of a Starter month. Every unnecessary word has a price.

**The script has to sound spoken, not written.** Text that reads fine on a page sounds stilted from a mouth, and synthetic delivery amplifies that rather than hiding it.

## Word count by duration

Natural speaking pace is roughly 2.3-2.6 words per second. Plan against the lower end — synthetic voices that rush sync worse.

| Duration | Words | Credits | Fits |
|---|---|---|---|
| 8s | ~20 | 24 | One claim, or a hook |
| 12s | ~30 | 36 | Hook plus one point |
| 15s | ~38 | 45 | Problem, then solution |
| 20s | ~50 | 60 | Three tight beats |
| 30s | ~75 | 90 | A full short ad. The cap |

**Write the script, read it aloud with a timer, then cut.** Not estimate — actually time it. Everyone overestimates what fits.

## Cut these first

The words that always go, in order:

1. **Greetings.** "Hi everyone", "Hey guys" — two seconds of nothing
2. **Self-introduction.** Nobody asked yet
3. **Meta-narration.** "In this video I'm going to explain..."
4. **Brand name repetition.** Once is enough
5. **Qualifiers.** "Basically", "essentially", "actually", "really", "just"
6. **Setup for the point.** Start at the point
7. **Anything the visual already shows**

A 30-second script cut of those is usually a 15-second script that says the same thing — and costs half.

## Structure

```
0-2s    HOOK        the claim, the question, or the problem. No warm-up
2-6s    STAKES      why it matters to this person
6-12s   SUBSTANCE   the actual point, or the mechanism
12-15s  PROOF       a number, a specific, a result
15-18s  ACTION      one thing to do
```

**Start on the point.** The first sentence out of the mouth should be the thing that makes someone stay. Compare:

```
Weak:   "Hi, I'm Sarah from Acme, and today I want to talk to you
         about a problem a lot of people have with their kitchen
         worktops." (14s of nothing)

Strong: "Coffee rings on oak aren't stains. They're the finish
         lifting — which is why scrubbing makes it worse." (7s,
         and you have their attention)
```

## Write for the mouth

Read every line aloud. If you wouldn't say it to someone across a table, rewrite it.

| Written | Spoken |
|---|---|
| "It is important to note that..." | "Worth knowing —" |
| "utilise" | "use" |
| "in order to" | "to" |
| "approximately" | "about" |
| "Additionally," | "Also," / nothing |
| "This allows you to..." | "So you can..." |
| "There are three key factors" | "Three things matter" |

**Use contractions.** Their absence is the fastest way to sound like a press release being read out. "It's", "you'll", "doesn't", "here's".

**Short sentences.** One idea per sentence. Long subordinate clauses lose both the listener and the sync.

**Read the numbers as spoken.** "Twenty-nine euros a month", not "€29/mo". The voice has to say it.

## Write for the sync

Things that specifically degrade lip-sync quality:

- **Very fast delivery.** Rushed speech syncs worse. Write fewer words rather than speaking faster
- **Tongue-twisters and dense consonant clusters.** "Sixth strength" is hard for a human and worse for a model
- **Mumbled plosives.** P, B, M produce visible mouth shapes; if the audio is soft on them the mouth looks wrong
- **Long unbroken sentences.** Natural pauses give the sync something to land on
- **Numbers and acronyms read as characters.** "A-P-I" needs to be articulated
- **Whispers, shouts and heavy emotion.** Extremes sync worse than a level delivery

Write in short sentences with real pauses. It's better speech and it's better sync.

## Segment past 30 seconds

The cap is hard. Longer scripts get generated in pieces.

```
Segment 1  0-14s   hook + problem       42 credits
Segment 2  14-28s  solution + proof     42 credits
Segment 3  28-38s  offer + CTA          30 credits
```

Two rules make the segments cut together:

- **Break on natural pauses**, never mid-sentence
- **Same face image and framing** across all segments

Then cover the joins with b-roll rather than accepting a visible cut. A two-second product cutaway makes the seam disappear. See `b-roll-generation` and `lip-sync-spokesperson-video`.

## Script the visuals alongside

A talking head with nothing else is the weakest use of the format. Plan the cutaways while writing.

```
0-2s    "Coffee rings on oak aren't stains."        FACE
2-6s    "They're the finish lifting."               B-ROLL: close-up of the ring
6-12s   "Which is why scrubbing makes it worse."    B-ROLL: cloth, wrong way
12-15s  "This lifts it in one pass."                B-ROLL: product, one pass
15-18s  "Link's below."                             FACE
```

That's 18 seconds of talking-head cost only on the face segments — the b-roll is generated separately and far cheaper. It's also a better video.

## The honest question

Before scripting: **does this need a person at all?**

A talking head earns 3 credits/second when the message needs a human — trust, testimony, explanation, personality. It's waste when someone is narrating what the camera could show.

- **Demonstration** → show it. `product-demo-video`
- **Transformation** → show it. `before-after-transformation-video`
- **A list or facts** → captions over b-roll, at a fraction of the cost
- **Anything visual** → the visual

## Don't

- **Don't estimate the timing.** Read it aloud with a timer.
- **Don't open with a greeting or an introduction.**
- **Don't write without contractions.**
- **Don't write long sentences.** They lose the listener and the sync.
- **Don't write numbers as symbols.** Write them as spoken.
- **Don't speak faster to fit.** Cut words.
- **Don't break a segment mid-sentence.**
- **Don't script a talking head** for something the visual could show.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`lip-sync-spokesperson-video`, `ai-avatar-presenter`, `b-roll-generation`, `video-duration-pacing`, `video-credit-cost-management`, `ugc-script-writing`
