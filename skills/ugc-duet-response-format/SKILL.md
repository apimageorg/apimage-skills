---
name: ugc-duet-response-format
description: Produce duet, stitch and split-screen response video, and understand what you may legally react to. Use whenever the user wants duet or stitch content, split-screen reactions, myth-busting responses, or wants to react to someone else's video.
---

# Duet and Response Format

Reacting to someone else's video borrows their attention, which is why the format performs. It also means you're publishing someone else's content inside an advertisement, which is where it gets complicated.

The production side is simple: a **9:16 half-frame** generated to sit beside existing footage. The rights side needs deciding before you generate anything.

## What you may react to

| Source | In organic content | In a paid ad |
|---|---|---|
| Your own earlier video | Yes | Yes |
| A customer's video, with written permission | Yes | Yes, with permission |
| A creator's video, with a paid usage licence | Yes | Yes, per the licence |
| A public video, via the platform's duet feature | Usually, per platform terms | **Usually not** |
| A public video, re-uploaded by you | **No** | **No** |
| A competitor's ad | **No** | **No** |
| Anything with music you don't have rights to | No | No |

**The line that catches people:** the platform's duet feature grants a licence for on-platform duetting under its terms. It does not grant you the right to put that person's face in a paid advertisement. Boosting a duet often crosses from organic to advertising, and the person you duetted never agreed to appear in an ad.

**If it's going into paid, use your own footage or licensed footage.** That's the whole rule.

## The formats

```
Duet          side-by-side, both play together        reaction, contrast
Stitch        their clip first, then yours             myth-busting, answering
Split-screen  your own two clips, self-hosted          before/after, comparison
Green screen  you in front of their content or a still commentary
```

**Split-screen with your own footage is the version with no rights problem at all** — and it does most of the same work. Two of your clips side by side: the wrong way and the right way, the old product and the new, the claim and the demonstration.

## Generating a duet half

The technical requirement: your half has to be framed for half a 9:16 frame, which is a much tighter crop than usual.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=CREATOR + [PRODUCT],
  prompt="She is already mid-sentence, close to camera, reacting "
         "with a slight sceptical head shake. Composed tight so "
         "the subject fills the frame with head and shoulders "
         "only, centred, nothing important near the edges. "
         "Handheld phone footage, slight shake, natural window "
         "light. Unpolished, shot on a phone. Not studio.",
  aspect_ratio="9:16", resolution="hd", duration=6, seed=4271
)
```

Three clauses earning their place:

- **"Head and shoulders only"** — a wide shot is unreadable at half size
- **"Centred, nothing important near the edges"** — the duet crop takes the sides
- **"Already mid-sentence"** — a duet has no room for a lead-in

Generate at full 9:16 and let the platform crop; don't try to pre-compose the split. See `aspect-ratio-strategy`.

## The stitch structure

Stitch is the more useful of the two, because it's sequential rather than simultaneous — the viewer can actually follow it.

```
0-3s     THEIR CLIP     the claim, the question, the mistake
3-5s     THE TURN       "that's half right" or "that's the bit
                        everyone gets wrong"
5-14s    THE CORRECTION demonstrated
14-17s   THE POINT
17-18s   ONE ACTION
```

**"That's half right" outperforms "that's wrong."** Flat contradiction triggers defence in the viewer as well as the original poster; partial agreement then correction is heard.

Keep their clip short. Three seconds is plenty and it's also the fair-use-friendly amount — you're commenting on it, not republishing it.

## Myth-busting is the strongest use

The response format's best fit is correcting a widely-believed thing in your category.

```
Their clip:  "Just sand the ring out and re-oil it"
Your turn:   "Sanding works, and it's also how people end up with
              a pale patch that never matches. Here's why."
```

You get the borrowed attention, a genuinely useful correction, and a natural product placement — without claiming any personal experience, which keeps a generated presenter on safe ground. See `ugc-tutorial-demo` and `ugc-disclosure-compliance`.

**Don't punch down.** Correcting a small creator's video from a brand account reads badly and often produces a worse comment section than the reach is worth. React to claims, categories and widely-shared ideas, not to individuals.

## The self-contained alternative

If rights are unclear — and they usually are — build the format from your own material.

```
Left / first:   your own earlier clip, or the untreated surface,
                or the common mistake demonstrated
Right / second: the correction, same framing, same light, same seed
```

Same visual grammar, same pacing, no licence question, and it can go straight into paid. Lock the seed and look clause across both halves so they read as one piece. See `before-after-transformation-video` and `seed-locked-iteration`.

## Don't

- **Don't put someone else's face in a paid ad** without a licence.
- **Don't re-upload someone's video.** Use the platform feature or nothing.
- **Don't react to a competitor's ad.**
- **Don't boost a duet** made under organic platform terms.
- **Don't frame a duet half wide.** Head and shoulders.
- **Don't open with a lead-in.** There's no room.
- **Don't flatly contradict.** "Half right" lands better.
- **Don't punch down** at individual small creators.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-comment-reaction-format`, `ugc-legal-likeness-rights`, `aspect-ratio-strategy`, `before-after-transformation-video`, `ugc-tutorial-demo`, `trend-format-replication`
