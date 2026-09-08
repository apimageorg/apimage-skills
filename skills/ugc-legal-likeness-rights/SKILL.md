---
name: ugc-legal-likeness-rights
description: Handle likeness, voice, publicity and usage rights when generating people or reusing creator content. Use whenever the user generates a person for commercial use, wants to recreate a real person, licenses creator footage, or asks whether they can use someone in an ad.
---

# Likeness, Voice and Usage Rights

Generating a person for an advertisement raises questions that don't come up when you hire one, because the release form is where those questions normally get answered. Nobody signs a release for a generated face.

This is not legal advice. It's the set of questions to resolve, and the defaults that keep you out of trouble.

## The four separate rights

They're often conflated, and they fail independently.

| Right | Protects | Applies to |
|---|---|---|
| **Image and likeness / publicity** | A person's face and identifiable appearance | Real people, living and in some places deceased |
| **Voice** | A person's identifiable voice | Increasingly protected specifically |
| **Copyright** | The footage or photograph itself | Whoever shot it |
| **Performance and usage** | What a performer agreed to | Per the contract's scope |

A creator's Instagram post involves at least three of these. Having permission for one is not having permission for the others — which is the single most common mistake in this area.

## Never recreate a real person

The hard line, with no useful exceptions in a commercial context.

- **No celebrities.** Not as a "style", not as a "lookalike", not "in the manner of"
- **No public figures**, including politicians, executives and athletes
- **No deceased people.** Post-mortem publicity rights exist in many jurisdictions, and estates enforce them
- **No named private individuals**
- **No voice cloning** of an identifiable person without explicit written consent
- **No "inspired by"** a specific real person's appearance in a way that reads as them

Three reasons stacked: publicity rights are enforced and damages can be substantial; false endorsement is a separate claim; and platforms have specific synthetic-media rules with account-level consequences. See `video-brand-safety-moderation`.

**Uploading someone's photo as a reference image to generate an ad presenter is the version of this people don't recognise as a problem.** It is the same problem.

## Accidental resemblance

A real risk with generated faces, and mostly manageable.

- **Don't name anyone** in a prompt, including obliquely
- **Don't describe a specific person's distinctive features** as a composite
- **Reverse-image-search** any face you're about to use at scale. Cheap, and it catches the bad cases
- **If it looks like someone recognisable, discard it.** You have unlimited alternatives at a few credits each
- **Review before a large campaign**, not after

**Resemblance alone is generally not actionable; resemblance plus an implied endorsement is.** So the risk concentrates exactly where you're using the face — in an ad.

## Generated people you can rely on

The workable default for commercial creative.

```
[ ] Generated from a generic description, no real person referenced
[ ] Checked against reverse image search
[ ] Saved as a character brand asset so it stays the same person
[ ] Disclosed as AI-generated per platform rules
[ ] Makes no claim of personal experience
[ ] No name, no biography, no invented backstory
```

**Don't give a synthetic presenter a name and a life.** A named "customer" with a job and a hometown is a fabricated person presented as real, which invites exactly the misrepresentation problem that a disclosed presenter avoids. A presenter needs no identity beyond being a presenter. See `ugc-creator-persona-design` and `ugc-disclosure-compliance`.

## Licensing real creator content

If you're reusing footage a creator or customer made, get the scope right in writing.

```
Licence terms to specify:
  Channels        organic social, paid social, website, email, retail,
                  out of home
  Territories     which markets
  Duration        a fixed term. Not "perpetual" by default
  Paid usage      explicitly stated. The one most often missed
  Editing rights  may you cut, caption, re-voice, extend
  Whitelisting    running ads from their handle needs their
                  platform-side grant, separately
  Exclusivity     may they promote a competitor
  Renewal         what happens at expiry
```

Two failures that recur:

**"They posted it publicly" is not a licence.** Nor is tagging you, nor a platform's own terms — those grant the platform rights, not you.

**Organic permission does not cover paid.** Boosting a creator's post is a materially different use, and it's the most common breach in the whole area. See `ugc-affiliate-creative`.

**Diary the expiry.** Licences lapse and ads keep running. That's how a compliant campaign becomes an unlicensed one without anybody doing anything.

## Extending real footage with generated material

A capability worth being careful with: generating scenery around, before or after real footage of a real person.

| Acceptable | Not |
|---|---|
| Generated b-roll cut alongside their clip | Generating them doing something they didn't |
| Generated background behind a product they showed | Putting their face on generated motion |
| Extending a set they were photographed in | Generating additional dialogue for them |
| Colour and framing adjustments | Changing what they appear to endorse |

**The test: does the finished piece show them doing or saying anything they didn't?** If yes, that's a manipulated depiction of a real person, regardless of how the footage was licensed — and no editing clause covers it.

## Customer content and testimonials

- **Written permission**, naming advertising as the use. A support-ticket reply is not permission
- **Keep the verbatim, the date and the permission** together, on file, for as long as the ad runs
- **Don't generate a face** to attach to a real person's quote. Attribute the words; don't invent the person
- **Public reviews still need permission** for advertising use in most markets
- **Withdrawal.** Have a route to pull content when someone asks, and honour it promptly

See `ugc-testimonial-video`.

## Model and dataset terms

The layer people forget entirely.

- **Check the platform's terms** for commercial use of outputs, and keep a copy of the version you relied on
- **Copyright in generated output is unsettled** in several jurisdictions — you may have weaker rights in your own creative than you assume, which matters if you ever need to enforce against a copycat
- **Trade mark still applies.** Don't generate someone else's logo, packaging or trade dress
- **Keep provenance records:** model, prompt, references, date. When a complaint arrives, being able to show a face was generated from a generic description rather than a real person's photo is the difference between a short conversation and a long one

## Don't

- **Don't generate a real, identifiable person.** Any real person.
- **Don't use someone's photo as a reference** for an ad presenter.
- **Don't clone a voice** without written consent.
- **Don't give a synthetic presenter a name and backstory.**
- **Don't treat a public post as a licence.**
- **Don't boost organic-permission content** into paid.
- **Don't generate a real person doing something they didn't.**
- **Don't run past a licence expiry.** Diary it.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`ugc-disclosure-compliance`, `ugc-testimonial-video`, `ugc-affiliate-creative`, `ugc-street-interview-format`, `video-brand-safety-moderation`, `ugc-duet-response-format`
