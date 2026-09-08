---
name: ugc-script-writing
description: Write UGC ad scripts that sound like a person talking rather than marketing copy read aloud. Use whenever the user needs a UGC script, creator-style copy, a script for an AI presenter, or has a script that sounds written rather than spoken.
---

# Writing UGC Scripts

The most common failure in synthetic UGC isn't the visual. It's that the visual says "real person in a kitchen" and the words say "marketing department".

A UGC script has to sound like speech. That's a different craft from writing ad copy, and the tells are specific.

## Read it aloud. Every line.

If you wouldn't say it to someone across a table, rewrite it. That single test catches most of what's wrong.

| Written | Spoken |
|---|---|
| "This innovative formula delivers..." | "This actually works because..." |
| "It is important to note that" | "Worth knowing —" |
| "utilise" | "use" |
| "in order to" | "to" |
| "Additionally," | "Also," or nothing |
| "This allows you to" | "So you can" |
| "There are three key benefits" | "Three things" |
| "Simply apply the product" | "You just put it on" |
| "for a limited time only" | never say this |

**Contractions are non-negotiable.** "It's", "you'll", "doesn't", "here's", "I've". Their absence is the fastest way to sound like a press release being read out.

## Structure to the duration

Lip sync bills at **3 credits per second** with a **30-second cap**, so script length is a budget decision. Speaking pace is roughly 2.3-2.6 words per second — plan against the lower end.

| Duration | Words | Credits | Fits |
|---|---|---|---|
| 8s | ~20 | 24 | One claim or a hook |
| 12s | ~30 | 36 | Hook plus one point |
| 15s | ~38 | 45 | Problem, then solution |
| 20s | ~50 | 60 | Three tight beats |
| 30s | ~75 | 90 | A full short ad. The cap |

**Read it with a timer**, not an estimate. Everyone overestimates what fits.

And remember: only the **face beats** need lip sync. The b-roll beats carry the same voiceover at a fraction of the cost. See `talking-head-scripting` and `b-roll-generation`.

## The shape

```
0-3s    HOOK        the specific problem, or the claim. Mid-sentence
3-6s    STAKES      why it mattered
6-12s   PRODUCT     what it does, in plain words
12-16s  RESULT      what changed, specifically
16-18s  ACTION      one thing, casually
```

**Start on the point.** Cut every greeting, introduction and "in this video". Compare:

```
Weak:   "Hey guys, so today I wanted to talk to you about
         something I've been using in my kitchen." (8s of nothing)

Strong: "Coffee rings on oak aren't stains — it's the finish
         lifting. Which is why scrubbing makes it worse." (7s,
         and you have their attention)
```

## Specificity is the whole technique

Vague copy reads as advertising. Specific copy reads as experience.

| Vague | Specific |
|---|---|
| "works really well" | "took about ten seconds" |
| "great value" | "forty quid, lasts a year" |
| "so many people love it" | "my sister has two" |
| "amazing results" | "the ring was gone, the grain wasn't damaged" |
| "premium quality" | "the handle's solid metal, not coated plastic" |
| "life-changing" | delete this |

**The test:** could this exact sentence appear in a competitor's script with the product name swapped? If yes, it's saying nothing.

## Include a reservation

The single most persuasive thing a UGC script can do, and the thing brands resist.

```
"It's not cheap. But I'd rather pay once than replace a worktop."

"It doesn't work on varnished wood — I tried. Oil-finished only."

"The smell's strong for about ten minutes. Worth it."
```

A script with no downside reads as an advert, because adverts don't have downsides. One honest reservation makes everything else credible — and it pre-qualifies people the product isn't for, which reduces returns.

**This is also the safest way to be persuasive without over-claiming.** Naming a limitation is a substantiated statement.

## Write in the persona's register, not brand voice

Each creator persona speaks differently, and the script should change with them.

```
Creator A (practical, late thirties):
  "Coffee rings aren't stains. It's the finish lifting. This puts
   it back. Ten seconds, done."

Creator B (fast, mid twenties):
  "Okay so I thought my table was ruined? It wasn't. It was the
   finish. This fixed it in like ten seconds."

Creator C (measured, fifties):
  "I'd assumed the marks were permanent. They weren't — the finish
   had lifted, and this restored it."
```

Same information, three voices. Testing the same message across personas is often a larger performance variable than testing the message. See `ugc-creator-persona-design` and `ugc-multi-creator-variants`.

## What a synthetic presenter cannot say

The line that governs the whole script. A generated person has no experience, so they cannot report any.

**Not allowed:**
- "I've been using this for three months"
- "I bought this and..."
- "It worked for me"
- "My skin has completely changed"
- Any duration, purchase or personal result claim

**Allowed:**
- "Here's what this does"
- "The way this works is..."
- "It's designed for [X], not [Y]"
- "The claim is [X] — here's the demonstration"

**Write the script as a demonstration, not a testimony.** That's the difference between a compliant synthetic presenter and a false endorsement, and it's a scripting decision made at draft stage. See `ugc-disclosure-compliance`.

If you want genuine testimony, get it from a real customer with permission and generate the scenery around their footage.

## Write for the sync

Things that specifically degrade lip-sync quality:

- **Fast delivery.** Write fewer words rather than speaking faster
- **Dense consonant clusters.** Hard for a human, worse for a model
- **Long unbroken sentences.** Natural pauses give the sync something to land on
- **Numbers and acronyms as symbols.** Write "forty quid", not "£40"
- **Extremes of emotion.** Level delivery syncs better than shouting or whispering

Short sentences with real pauses. Better speech, better sync.

## Don't

- **Don't write without contractions.**
- **Don't open with a greeting or introduction.**
- **Don't estimate the timing.** Read it with a timer.
- **Don't write vague benefit language.** Specifics.
- **Don't omit a reservation.** It's what makes the rest credible.
- **Don't write in brand voice.** Write in the persona's.
- **Don't let a synthetic presenter claim experience.**
- **Don't write numbers as symbols.** They have to be spoken.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-hook-library`, `ugc-creator-persona-design`, `talking-head-scripting`, `ugc-disclosure-compliance`, `ugc-ad-video-generation`, `ugc-problem-solution-format`
