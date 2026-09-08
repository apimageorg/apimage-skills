# APImage Skills

Ninety agent skills for generating marketing video and imagery with [APImage](https://apimage.org) — viral and UGC ad video, product photography, spokesperson video, and the disclosure and rights questions that come with generating people, driven from Claude, Cursor, Codex or any MCP client.

```bash
npx skills add apimageorg/apimage-skills
```

Works with Claude Code, Claude.ai and Claude Desktop, plus 70+ other agents including Cursor, Codex, Copilot, Gemini CLI, Windsurf, OpenCode and Zed.

Maintained by [APImage](https://apimage.org). MIT licensed.

---

## Status

**Viral video: complete (30). Product photo: complete (30). UGC content: complete (30).** Ninety skills, all published. Suggestions and pull requests welcome — see [Adding a skill](#adding-a-skill).

## Connect APImage first

Every skill assumes the APImage MCP server is connected.

**Claude Code:**

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

**Codex**, via `mcp-remote`:

```toml
[mcp_servers.apimage]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.apimage.org/mcp", "--header", "Authorization: Bearer sk_your_api_key"]
```

Keys come from the [APImage dashboard](https://apimage.org). One endpoint, one bearer token, no SDK.

## The skills

### Workflow and cost

The four that change the economics of everything else. Read these first.

| Skill | What it prevents |
|---|---|
| [`draft-then-enhance-workflow`](skills/draft-then-enhance-workflow/SKILL.md) | Iterating at full quality — the most expensive habit on the platform |
| [`async-video-job-orchestration`](skills/async-video-job-orchestration/SKILL.md) | Re-calling `generate_video` because nothing came back, and paying twice |
| [`video-credit-cost-management`](skills/video-credit-cost-management/SKILL.md) | Burning a monthly quota in an afternoon |
| [`seed-locked-iteration`](skills/seed-locked-iteration/SKILL.md) | Rolling new dice every generation and learning nothing |

### Generating video

| Skill | Covers |
|---|---|
| [`video-model-selection`](skills/video-model-selection/SKILL.md) | Choosing between the five video models, and matching resolution to destination |
| [`text-to-video-prompting`](skills/text-to-video-prompting/SKILL.md) | Prompting motion and camera, not just a frame |
| [`image-to-video-animation`](skills/image-to-video-animation/SKILL.md) | Animating a real product photo instead of letting the model invent one |
| [`video-to-video-restyling`](skills/video-to-video-restyling/SKILL.md) | Restyling existing footage, where the motion is inherited rather than invented |
| [`multi-scene-video-assembly`](skills/multi-scene-video-assembly/SKILL.md) | Generating beats and cutting them, rather than one long drifting clip |
| [`video-duration-pacing`](skills/video-duration-pacing/SKILL.md) | Duration as a budget for beats, and cut rhythm |
| [`character-consistency-video`](skills/character-consistency-video/SKILL.md) | Brand assets, so the presenter and product stay the same across a campaign |
| [`ai-avatar-presenter`](skills/ai-avatar-presenter/SKILL.md) | Building a reusable synthetic presenter you own the rights to |
| [`lip-sync-spokesperson-video`](skills/lip-sync-spokesperson-video/SKILL.md) | Talking avatars, the 30s cap and the 3-credits-per-second reality |
| [`talking-head-scripting`](skills/talking-head-scripting/SKILL.md) | Scripts that fit the duration and survive lip-sync |
| [`b-roll-generation`](skills/b-roll-generation/SKILL.md) | Cheap cutaways that cover joins and carry voiceover |
| [`batch-video-production`](skills/batch-video-production/SKILL.md) | Volume: the concurrency cap, webhooks, budget and reconciliation |

### Making it perform

| Skill | Covers |
|---|---|
| [`video-hook-first-3-seconds`](skills/video-hook-first-3-seconds/SKILL.md) | The opening that decides whether anything else is watched |
| [`aspect-ratio-strategy`](skills/aspect-ratio-strategy/SKILL.md) | Safe areas, and why the bottom third of a vertical video is unusable |
| [`video-caption-subtitle-planning`](skills/video-caption-subtitle-planning/SKILL.md) | Why text never goes in the render, and where subtitles actually belong |
| [`video-thumbnail-generation`](skills/video-thumbnail-generation/SKILL.md) | Designing for 200 pixels wide, not for full size |
| [`music-audio-pairing`](skills/music-audio-pairing/SKILL.md) | Sound design, and the audio licensing that catches people out |
| [`video-ab-testing-variants`](skills/video-ab-testing-variants/SKILL.md) | Testing one variable at a time so results are reusable |
| [`video-brand-safety-moderation`](skills/video-brand-safety-moderation/SKILL.md) | `safety_tolerance`, and why passing moderation isn't the same as brand-safe |

### Platforms and formats

| Skill | Covers |
|---|---|
| [`tiktok-video-generation`](skills/tiktok-video-generation/SKILL.md) | Native formatting, sound-off viewing, and platform AI rules |
| [`instagram-reels-generation`](skills/instagram-reels-generation/SKILL.md) | Where Reels rewards polish, and the feed-crop constraint |
| [`youtube-shorts-generation`](skills/youtube-shorts-generation/SKILL.md) | Search intent, longer tolerance, and titles that actually rank |
| [`faceless-video-automation`](skills/faceless-video-automation/SKILL.md) | Channels without a presenter, and what the platform policies target |
| [`product-demo-video`](skills/product-demo-video/SKILL.md) | Keeping the real product accurate while the scene moves |
| [`before-after-transformation-video`](skills/before-after-transformation-video/SKILL.md) | The highest-retention format, and where the claim line sits |
| [`trend-format-replication`](skills/trend-format-replication/SKILL.md) | Copying the structure, not the content or the audio |

## Product photography

### Tools and workflow

| Skill | Covers |
|---|---|
| [`image-model-selection`](skills/image-model-selection/SKILL.md) | The five image models, and why FLUX can't do text |
| [`product-photo-from-reference`](skills/product-photo-from-reference/SKILL.md) | The core loop: real photo in, accurate variations out |
| [`background-removal-workflow`](skills/background-removal-workflow/SKILL.md) | `remove_background` at 2 credits, and the white-and-black edge test |
| [`background-replacement-scenes`](skills/background-replacement-scenes/SKILL.md) | `replace_background` at 3 credits — it relights the subject |
| [`product-relighting`](skills/product-relighting/SKILL.md) | Changing light without losing the composition |
| [`camera-angle-variation`](skills/camera-angle-variation/SKILL.md) | Which angles are derivable, and which get invented |
| [`inpainting-product-fixes`](skills/inpainting-product-fixes/SKILL.md) | Masked repair instead of regenerating |
| [`product-upscaling-4k`](skills/product-upscaling-4k/SKILL.md) | Meeting zoom thresholds, and what upscaling can't recover |
| [`product-photo-consistency`](skills/product-photo-consistency/SKILL.md) | Brand assets and verbatim look clauses across a catalogue |
| [`product-photo-batch-pipeline`](skills/product-photo-batch-pipeline/SKILL.md) | Catalogue scale: rate limits, budget, reconciliation |
| [`product-image-qa-review`](skills/product-image-qa-review/SKILL.md) | The artefacts and accuracy failures that pass a glance |

### Shot types

| Skill | Covers |
|---|---|
| [`white-background-ecommerce`](skills/white-background-ecommerce/SKILL.md) | Exact 255,255,255, 85% fill, and the shadow rules |
| [`lifestyle-product-photography`](skills/lifestyle-product-photography/SKILL.md) | Context, and why imperfection reads as real |
| [`flat-lay-composition`](skills/flat-lay-composition/SKILL.md) | Overhead composition, negative space, prop discipline |
| [`product-in-hand-shots`](skills/product-in-hand-shots/SKILL.md) | Scale and use — and giving the model less hand to get wrong |
| [`model-wearing-product`](skills/model-wearing-product/SKILL.md) | On-model, with the likeness and fit constraints |
| [`product-detail-macro`](skills/product-detail-macro/SKILL.md) | Raking light, and why stitch quality must be photographed |
| [`multi-product-scene`](skills/multi-product-scene/SKILL.md) | Range shots — count them, and measure relative scale |
| [`packaging-mockup-generation`](skills/packaging-mockup-generation/SKILL.md) | Generate the form, overlay the artwork. Never the copy |
| [`product-scale-reference`](skills/product-scale-reference/SKILL.md) | Cutting size-driven returns |
| [`seasonal-product-styling`](skills/seasonal-product-styling/SKILL.md) | One master, every season, at 3 credits a scene |

### Channels and categories

| Skill | Covers |
|---|---|
| [`marketplace-image-compliance`](skills/marketplace-image-compliance/SKILL.md) | The rules across marketplaces, and AI disclosure |
| [`amazon-listing-images`](skills/amazon-listing-images/SKILL.md) | Main image spec, and what each gallery slot is for |
| [`shopify-product-images`](skills/shopify-product-images/SKILL.md) | Collection consistency, variant images, page weight |
| [`etsy-listing-photos`](skills/etsy-listing-photos/SKILL.md) | Where polish actively hurts |
| [`social-commerce-product-images`](skills/social-commerce-product-images/SKILL.md) | Native beats catalogue-clean |
| [`apparel-product-photography`](skills/apparel-product-photography/SKILL.md) | Ghost mannequin, fabric, and colour as the return driver |
| [`food-beverage-photography`](skills/food-beverage-photography/SKILL.md) | Appetising, and the regulatory line on portions |
| [`jewelry-reflective-products`](skills/jewelry-reflective-products/SKILL.md) | The hardest category — halos, reflections, added sparkle |
| [`furniture-room-scenes`](skills/furniture-room-scenes/SKILL.md) | Scale anchors, perspective, and possible light |

## UGC content

### The format, and the line

| Skill | Covers |
|---|---|
| [`ugc-ad-video-generation`](skills/ugc-ad-video-generation/SKILL.md) | The beat structure, the production loop, prompting against polish |
| [`ugc-vs-polished-decision`](skills/ugc-vs-polished-decision/SKILL.md) | Where rough wins, and where it actively hurts |
| [`ugc-authenticity-signals`](skills/ugc-authenticity-signals/SKILL.md) | The clause library, and the signals that go too far |
| [`ugc-disclosure-compliance`](skills/ugc-disclosure-compliance/SKILL.md) | Two separate obligations: AI labelling and endorsement law |
| [`ugc-legal-likeness-rights`](skills/ugc-legal-likeness-rights/SKILL.md) | Likeness, voice, licences, and what a paid usage grant must say |
| [`ugc-diversity-representation`](skills/ugc-diversity-representation/SKILL.md) | The model's default is narrow. Unspecified means default |

### Creators and scripts

| Skill | Covers |
|---|---|
| [`ugc-creator-persona-design`](skills/ugc-creator-persona-design/SKILL.md) | Design from the audience, not from aesthetics |
| [`ugc-character-consistency`](skills/ugc-character-consistency/SKILL.md) | Seeds give a look; assets give a person |
| [`ugc-script-writing`](skills/ugc-script-writing/SKILL.md) | Read it aloud, and include a reservation |
| [`ugc-hook-library`](skills/ugc-hook-library/SKILL.md) | Twelve patterns, and why the log is the asset |
| [`ugc-multi-creator-variants`](skills/ugc-multi-creator-variants/SKILL.md) | Who delivers it beats what they say |

### Video formats

| Skill | Covers |
|---|---|
| [`ugc-problem-solution-format`](skills/ugc-problem-solution-format/SKILL.md) | The highest-converting structure, usually built backwards |
| [`ugc-tutorial-demo`](skills/ugc-tutorial-demo/SKILL.md) | Teach the method, not the product |
| [`ugc-testimonial-video`](skills/ugc-testimonial-video/SKILL.md) | Real words, generated scenery |
| [`ugc-review-format`](skills/ugc-review-format/SKILL.md) | Criteria instead of a verdict, and disqualifying yourself |
| [`ugc-unboxing-video`](skills/ugc-unboxing-video/SKILL.md) | Photograph the packaging, and the hands QA pass |
| [`ugc-asmr-sensory-format`](skills/ugc-asmr-sensory-format/SKILL.md) | The best fit for generation: no face, no claims |
| [`ugc-day-in-the-life`](skills/ugc-day-in-the-life/SKILL.md) | Light describes the time. Product in half the beats |
| [`ugc-get-ready-with-me`](skills/ugc-get-ready-with-me/SKILL.md) | Order of application, and real skin |
| [`ugc-haul-format`](skills/ugc-haul-format/SKILL.md) | Ten items, and eight price claims on screen |
| [`ugc-comment-reaction-format`](skills/ugc-comment-reaction-format/SKILL.md) | Your audience writes the hook. Never fabricate the comment |
| [`ugc-duet-response-format`](skills/ugc-duet-response-format/SKILL.md) | The half-frame crop, and what you may react to |
| [`ugc-street-interview-format`](skills/ugc-street-interview-format/SKILL.md) | The worst format to generate, and the quote reel instead |

### Stills and commerce

| Skill | Covers |
|---|---|
| [`ugc-selfie-style-photos`](skills/ugc-selfie-style-photos/SKILL.md) | Deep focus is the clause that kills the studio look |
| [`ugc-lifestyle-photos`](skills/ugc-lifestyle-photos/SKILL.md) | An ordinary room, and matching it to the buyer |
| [`ugc-photo-dump-format`](skills/ugc-photo-dump-format/SKILL.md) | Vary the seed on purpose. Keep the weak image |
| [`ugc-tiktok-shop-content`](skills/ugc-tiktok-shop-content/SKILL.md) | No landing page, so the video answers the objection |
| [`ugc-affiliate-creative`](skills/ugc-affiliate-creative/SKILL.md) | Burn the disclosure into the asset |

### Testing

| Skill | Covers |
|---|---|
| [`ugc-batch-testing`](skills/ugc-batch-testing/SKILL.md) | Sequential beats factorial, and draft everything first |
| [`ugc-performance-iteration`](skills/ugc-performance-iteration/SKILL.md) | Read the metric, fix the beat, not the video |

## What the skills are built on

They encode the parts of APImage that aren't obvious from the tool list:

- **Video is always asynchronous.** `generate_video` returns a job ID. Poll with `get_video_generation` — polling is free, regenerating is not.
- **`flux-3-video-draft` → `enhance_video_draft`** lets you iterate cheaply and pay full price once. This changes project cost by a large multiple.
- **Failed renders are already refunded.** No reconciliation, no defensive retries.
- **Lip sync is `seedance-2-0` only**, bills at 3 credits/second, and caps at 30 seconds.
- **Brand assets** — character, product, background, preset — are how consistency works. Seeds give a consistent *look*, not a consistent *subject*.
- **Everything diagnostic is free**: `enhance_prompt`, all polling, all history, all brand asset management, `check_credits`.
- **Errors carry the upstream HTTP status**, so a 400 or 422 is deterministic and shouldn't be retried unchanged.

## A recurring theme

Several skills say the same thing in different contexts, because it's the difference between output that performs and output that reads as generated:

**The model's default is commercial polish, and commercial polish is what feeds skip.** Studio lighting, smooth camera moves, centred products and even exposure all have to be prompted *against* — explicitly, with negative framing — to produce something that looks native.

## Disclosure and rights

The skills take a position rather than staying quiet:

- **Don't use a real person's likeness** without permission, including public figures and stock portraits whose licence excludes synthetic use. Generate a presenter instead.
- **Don't present a synthetic person as a real customer.** That's a false endorsement, and an advertising-standards problem in many jurisdictions independent of platform policy.
- **Label AI content** where the platform requires it. TikTok and others apply their own detection and can limit distribution on undisclosed synthetic media.
- **Generated visuals don't get a lower bar for claim substantiation.** A clip demonstrating something the product doesn't do is a false claim however it was produced.

## Adding a skill

Copy [`docs/skill-template.md`](docs/skill-template.md), match `name:` to the directory name, add a row above, and verify with `npx skills add ./ --list`. House rules are in the template.

## License

MIT. See [LICENSE](LICENSE).
