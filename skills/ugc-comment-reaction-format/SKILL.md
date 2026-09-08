---
name: ugc-comment-reaction-format
description: Turn real comments and customer questions into reply-format short-form video, the cheapest reliable content engine a brand has. Use whenever the user needs a repeatable content format, wants to answer objections on video, or has comments and questions they aren't using.
---

# Comment Reply Format

The best-value format in short-form: your audience writes the hook for you. A real comment on screen is a pre-validated opening, the objection it raises is one somebody actually has, and the whole thing is legitimate to generate because you're answering a question rather than claiming an experience.

It is also close to infinite. Every support ticket is an episode.

## Why it works

- **The comment is the hook**, and it's tested by having been written
- **Objection handling at scale.** The thing that blocks purchase gets answered publicly
- **Genuinely repeatable.** No new concept needed each time
- **No fabricated experience.** You're answering, not testifying
- **Platform-native.** Reply-to-comment is a first-class feature on TikTok and Reels

The one input required is a real comment. Which brings the only hard rule.

## Never fabricate the comment

Inventing a comment is fabricating an audience member, and it's the same category of problem as a fake review. It's also unnecessary: you have more real questions than you have time to answer.

Sources, in order of usefulness:

```
Support tickets and email replies    the richest, and already yours
Product-page and marketplace Q&A
Social comments and DMs
Live-chat transcripts
Sales-call objections
Review text, especially critical
```

Screenshot or quote the real comment. If you need to anonymise, **blur the handle rather than inventing one.** A blurred handle reads as respectful; a fictional handle is a fake person.

**Answer the awkward ones.** "Why is this so expensive" outperforms "does it come in blue," because the awkward comment is the one everyone else is silently thinking.

## The structure

```
0-2s     THE COMMENT     on screen, read aloud, verbatim
2-4s     TAKE IT SERIOUSLY   concede the fair part
4-12s    THE ANSWER      specific, demonstrated where possible
12-16s   THE LIMIT       where the comment is right
16-18s   ONE ACTION
```

Two beats brands skip, and they're the ones that make it work.

**Concede the fair part:**

```
Comment:  "£40 for a bottle of oil is ridiculous"
Weak:     "Actually it's great value because..."
Strong:   "It is a lot for a bottle of oil. Here's the maths, and
           here's when you shouldn't buy it."
```

**Name the limit.** Answering an objection while admitting where it holds is the most credible thing a brand does on video. See `ugc-script-writing`.

## Producing it

Structurally simple: comment on screen, presenter or hands answering, product demonstrated where relevant.

```python
CREATOR = ["<char ref 1>", "<char ref 2>", "<char ref 3>"]
PROD    = "<product asset>"
LOOK    = ("Handheld phone footage, slight shake, slightly uneven "
           "exposure. Natural window light from the left, no fill. "
           "Ordinary kitchen with some clutter. Unpolished, shot "
           "on a phone. Not studio, not commercial.")

# face beat, needs lip sync
FACE = ("She is already mid-sentence, looking at the camera, "
        "slight nod as if conceding a point. Close, subject fills "
        "frame.")

# answer beat, no face, no lip sync cost
DEMO = ("A hand tips a small amount from the bottle onto a cloth "
        "and presses it to the worktop. Camera locked, close.")
```

The cost structure matters here. **Lip sync bills at 3 credits per second with a 30-second cap** — so sync only the face beats and carry the same voiceover over the demo beats. On a series of twenty replies that difference compounds. See `lip-sync-spokesperson-video` and `b-roll-generation`.

Fix the creator, background, look and seed once, and every subsequent episode is one prompt. Brand assets are free. See `ugc-character-consistency`.

## Run it as a series

The format's value is in volume, and volume needs a queue rather than inspiration.

```
1. Log every incoming question in one place, with a count
2. Sort by frequency, not by how easy it is to answer
3. Batch-generate 5-10 replies in one session as drafts
4. Enhance the ones worth publishing
5. Publish on a cadence, one per day or two
6. Feed replies to the replies back into the queue
```

**Sort by frequency.** The question asked forty times is worth a video even if it's boring; the interesting question asked once isn't.

`list_video_generations(status="completed")` is free and paginated — use it to keep track of a batch rather than your notes. See `ugc-batch-testing` and `batch-video-production`.

## Disclosure and accuracy

- **Brand account, brand answer.** Disclose on screen if the presenter is generated, per platform AI-labelling rules
- **The comment must be real and quoted accurately.** Don't edit it into a weaker objection
- **The answer is an advertising claim**, subject to the same substantiation as any other
- **Don't answer a medical, legal or financial question** with a generated presenter and no qualification
- **Don't quote a named individual's comment** in an ad without permission, even a public one

**Editing a comment to make it easier to answer is the sneaky failure** here. Quote it as written, including the rude bit.

## Don't

- **Don't invent the comment.** Ever.
- **Don't invent a handle** when anonymising. Blur it.
- **Don't edit the comment** to soften the objection.
- **Don't only answer easy questions.**
- **Don't skip the concession** or the limit.
- **Don't lip sync the whole video.** Face beats only.
- **Don't run it ad hoc.** It's a queue and a cadence.
- **Don't answer regulated questions** with a synthetic presenter.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-duet-response-format`, `ugc-script-writing`, `batch-video-production`, `lip-sync-spokesperson-video`, `ugc-review-format`, `ugc-batch-testing`
