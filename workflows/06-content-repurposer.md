# Content Repurposer Workflow

> **Purpose:** Turn one big idea into 10+ platform-specific content pieces that drive engagement across Colin's entire content ecosystem. Maximum leverage from minimum input.
> **Assigned Agent:** `Agent ContentRepurposer` (or any agent with the `content-repurposing` skill + `ai-voice-humanizer` skill + `linkedin-post-writing` skill)
> **Category:** Content / Growth Engine

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Colin Input** | Colin drops a blog post, podcast transcript, article, or raw idea into the `#content-ideas` channel | On demand |
| **Weekly Content Sprint** | Every Monday, agent scans Colin's recent activity (new blog posts, podcast appearances, conference talks) for repurposing opportunities | Weekly |
| **Trend Hijack** | Trend Researcher agent identifies a trending topic relevant to Colin's expertise; Colin approves an angle | On demand |
| **Event Pipeline** | Post-event: Colin's talk, panel, or workshop transcript is uploaded | Post-event |
| **Repurposing Queue** | Content backlog in Airtable has items > 7 days old waiting for repurposing | Weekly sweep |
| **Performance Signal** | Analytics agent identifies a high-performing piece of content that deserves amplification | On demand |

**Auto-run threshold:** Weekly content sprint runs automatically. All other triggers require Colin's explicit input or approval before execution.

---

## Input Format

### Required Fields
| Field | Description | Source |
|-------|-------------|--------|
| `source_content` | The raw content to repurpose: full text, transcript, URL, or bullet summary | Colin's input or auto-detected |
| `content_type` | `blog-post`, `podcast-transcript`, `article`, `video-script`, `talk-transcript`, `raw-idea` | Colin's input or auto-detected |
| `primary_angle` | The core insight or takeaway Colin wants to emphasize | Colin's input or agent-derived from source |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `target_platforms` | Which platforms to generate for: `all`, `linkedin`, `x`, `instagram`, `newsletter`, `blog`, `youtube` | `all` |
| `tone` | `sharp`, `consultative`, `storytelling`, `contrarian`, `practical` | Auto-detected from source content |
| `audience` | `sales-leaders`, `founders`, `ai-curious`, `wellness-professionals`, `general` | `general` |
| `colin_context` | What Colin is currently building or focused on | From `MEMORY.md` |
| `promotional_intent` | `none`, `soft` (mention [Your Brand] or AI Revenue Systems subtly), `direct` (clear CTA) | `soft` |
| `excluded_platforms` | Platforms to skip | `null` |
| `content_length` | `short` (1-2 posts), `medium` (5-8 posts), `full` (10+ posts) | `full` |

---

## Step-by-Step Process

### Phase 1: Content Ingestion & Decomposition
- [ ] **1.1 Load Source Content**
  - If URL: Use `defuddle` to extract clean markdown
  - If transcript: Clean up speaker labels, filler words, and timestamps
  - If raw idea: Expand into a structured outline with key points
  - If blog post: Extract headings, key stats, and quotable lines

- [ ] **1.2 Core Insight Extraction**
  - Identify the single most important insight from the source content
  - Ask: "If someone only remembered one thing from this, what should it be?"
  - Extract 3-5 supporting points that prove or illustrate the core insight
  - Extract 2-3 quotable lines or "mic drop" moments
  - Extract any data, stats, or frameworks mentioned

- [ ] **1.3 Angle Validation**
  - Cross-check `primary_angle` with the extracted core insight
  - If they conflict, flag for Colin to choose which angle to emphasize
  - If `primary_angle` is not specified, use the core insight as the default angle

### Phase 2: Platform-Specific Adaptation
- [ ] **2.1 Platform Analysis**
  - For each target platform, determine:
    - Optimal format (carousel, text post, thread, short-form video script, story)
    - Optimal length (character limits, attention spans, consumption patterns)
    - Optimal hook style (question, stat, contrarian take, story opener)
    - Optimal CTA (engage, click, follow, comment, share)
    - Best time to post (based on Colin's analytics history)

- [ ] **2.2 Content Generation Per Platform**

  #### LinkedIn (2-3 posts)
  - **Post 1: The Insight Post**
    - Format: 3-5 short paragraphs, bold key takeaways, no links in body (comment link)
    - Hook: "I spent 15 years in SaaS sales. Here's what nobody told me about [topic]."
    - Body: Core insight + 2-3 supporting points + personal anecdote
    - CTA: "What's your experience with this? Drop a comment."
    - Length: 150-300 words
  - **Post 2: The Story Post**
    - Format: Narrative arc, beginning → middle → lesson → question
    - Hook: Story opener: "In 2019, I made a decision that cost me £20,000. Here's what happened."
    - Body: Personal story that illustrates the core insight
    - CTA: "Has this happened to you?"
    - Length: 200-350 words
  - **Post 3: The Contrarian Take** (optional)
    - Format: Bold statement + evidence + counter-argument + synthesis
    - Hook: "Everyone says [common belief]. They're wrong. Here's why."
    - Body: Challenge a conventional wisdom, use data or logic
    - CTA: "Agree or disagree? Tell me why."
    - Length: 150-250 words

  #### X/Twitter (3-4 posts)
  - **Post 1: The Thread**
    - Format: 8-12 tweets, each 1-2 lines, visual spacing, one idea per tweet
    - Hook Tweet: "I spent 15 years in SaaS sales. Here's what nobody told me about [topic]:"
    - Body Tweets: Core insight broken into digestible steps or points
    - Final Tweet: CTA + link (if applicable)
    - Style: Punchy, quotable, high shareability
  - **Post 2: The Standalone Tweet**
    - Format: One powerful sentence or stat
    - Example: "The average SaaS sales rep spends 60% of their time on admin. AI can cut that to 10%. The math is simple. The execution is hard."
  - **Post 3: The Engagement Question**
    - Format: One question that sparks replies
    - Example: "What's the most underrated AI tool for sales teams right now? I'll start: [tool]."
  - **Post 4: The Visual/Quote Tweet** (optional)
    - Format: Screenshot of a key stat or framework + short commentary

  #### Instagram (2-3 posts)
  - **Post 1: The Carousel**
    - Format: 5-7 slides, each with one key point, bold typography, minimal text
    - Slide 1: Hook (problem statement or bold claim)
    - Slides 2-6: Supporting points, one per slide
    - Slide 7: CTA + Colin's handle
    - Design: Canva template, consistent brand colors
  - **Post 2: The Reel Script**
    - Format: 30-60 second spoken script
    - Hook (0-3 sec): "Stop doing [old way]. Here's the new way."
    - Body (3-50 sec): Core insight explained visually or with text overlays
    - CTA (50-60 sec): "Follow for more. Link in bio."
  - **Post 3: The Story**
    - Format: 3-5 story slides, poll or question sticker
    - Quick insight + engagement prompt

  #### Newsletter (1 issue)
  - Format: 500-800 words, structured like a mini-article
  - Subject line: Curiosity-driven or benefit-driven
  - Opening: Personal hook or story
  - Body: Core insight expanded with examples, frameworks, and actionable advice
  - Section: "What I'm building" (soft [Your Brand] or AI Revenue Systems mention)
  - CTA: Reply with thoughts, share with a friend, or check out a resource

  #### Blog Post (1 derivative post)
  - Format: 800-1,500 words, SEO-optimized
  - Title: Keyword-rich but human-readable
  - Structure: H2 sections, bullet points, internal links to other posts
  - Expand one supporting point from the source into a full article
  - Include a CTA to subscribe to the newsletter or follow on LinkedIn

  #### YouTube Short Script (1)
  - Format: 30-60 second script, vertical video optimized
  - Hook: Visual + verbal hook in first 3 seconds
  - Body: One insight explained with a simple framework or analogy
  - CTA: "Subscribe for more AI revenue systems."

### Phase 3: Colin's Voice & Brand Alignment
- [ ] **3.1 AI Voice Humanizer Pass**
  - Run every piece of content through `ai-voice-humanizer`
  - Check for: AI-sounding phrases, passive voice, generic filler, overuse of emojis, corporate speak
  - Ensure Colin's voice comes through: sharp, practical, quietly ambitious, 15y SaaS sales experience

- [ ] **3.2 Brand Safety Check**
  - If `promotional_intent` = `soft`:
    - [Your Brand] mentions should be natural (e.g., "What I'm building" section, not hard sell)
    - AI Revenue Systems should be framed as "what I do" not "hire me now"
  - If `promotional_intent` = `direct`:
    - Clear CTA: "Book a call", "Check out the product", "Join the newsletter"
  - Check for accidental claims (e.g., "guaranteed results" — remove unless substantiated)
  - Check for compliance: No medical claims for [Your Brand] unless reviewed by Compliance agent

- [ ] **3.3 Consistency Check**
  - Read all generated posts together. Do they tell the same story?
  - Check for contradictions: Does the LinkedIn post say X while the X thread says Y?
  - Check for repetition: Is the exact same sentence used across platforms? (If so, vary it.)
  - Ensure each post can stand alone — someone who sees only the Instagram post should still get value

### Phase 4: Quality & Delivery
- [ ] **4.1 Council Review**
  - Submit a sample of posts (1 per platform) to `kimi-council`
  - Council evaluates: Is this on-brand? Is the insight clear? Would this get engagement?
  - For [Your Brand]-related content: Compliance agent must also review for health claims

- [ ] **4.2 Colin Approval**
  - Present the full content package in `#content-repurposing` with:
    - A summary: "Here's what we generated from [source]"
    - Each post labeled by platform
    - Suggested posting schedule (spread across the week to avoid flooding)
    - A "posting queue" for Colin to approve/reject each piece
  - Colin can: approve all, edit any piece, reject any piece, or request a rewrite

- [ ] **4.3 Scheduling & Publishing**
  - Once approved, content is queued in Buffer (or Hypefury) for scheduled posting
  - LinkedIn and X: Auto-scheduled based on optimal times from analytics
  - Instagram: Content is prepared in Canva + caption ready; Colin or agent uploads manually (or via Buffer if API available)
  - Newsletter: Drafted in Substack, scheduled for Colin's review before sending
  - Blog: Drafted as markdown, ready for Colin to publish on his site or Medium
  - YouTube: Script ready, Colin records or agent generates a Loom-style video

---

## Output Format

```markdown
# Content Repurposing Package — [Source Title]
**Source Type:** [blog-post / podcast / etc.]  
**Primary Angle:** [Core insight]  
**Tone:** [sharp / consultative / etc.]  
**Promotional Intent:** [none / soft / direct]  
**Generated:** [ISO timestamp]  
**Package ID:** [Unique ID]

---

## 📊 Content Map

| Platform | Format | Length | Hook Style | CTA | Status |
|----------|--------|--------|------------|-----|--------|
| LinkedIn | Insight Post | 200 words | Experience-based | Comment engagement | Draft |
| LinkedIn | Story Post | 280 words | Story opener | Comment engagement | Draft |
| LinkedIn | Contrarian Take | 180 words | Challenge belief | Comment engagement | Draft |
| X | Thread | 10 tweets | Listicle hook | Link + follow | Draft |
| X | Standalone | 1 tweet | Stat punch | Engage | Draft |
| X | Question | 1 tweet | Open question | Reply | Draft |
| Instagram | Carousel | 7 slides | Problem statement | Follow + bio link | Draft |
| Instagram | Reel Script | 45 sec | Stop doing X | Follow | Draft |
| Instagram | Story | 4 slides | Quick tip | Poll | Draft |
| Newsletter | Full Issue | 650 words | Personal hook | Reply + share | Draft |
| Blog | Derivative Post | 1000 words | SEO title | Subscribe | Draft |
| YouTube | Short Script | 45 sec | Visual hook | Subscribe | Draft |

---

## 📝 LinkedIn — Insight Post

**[Full post text]**

**Character Count:** [N]  
**Optimal Post Time:** [Day + Time]  
**Estimated Engagement:** [High / Medium / Low] (based on historical performance of similar angles)

---

## 📝 X — Thread

**Tweet 1:** [Hook tweet]  
**Tweet 2:** [Point 1]  
...  
**Tweet 10:** [CTA tweet]

**Thread Length:** [N] tweets  
**Optimal Post Time:** [Day + Time]

---

## 📝 Instagram — Carousel Script

**Slide 1:** [Hook text]  
**Slide 2:** [Point 1]  
...  
**Slide 7:** [CTA text]

**Design Notes:** [Color scheme, font, image suggestions for Canva]

---

## 📝 Newsletter — Full Draft

**Subject Line:** [Subject]  
**Preview Text:** [Preview]

[Full newsletter text]

**Word Count:** [N]  
**Send Day:** [Tuesday / Thursday recommended]

---

## 📝 Blog — Derivative Post

**Title:** [SEO title]  
**Meta Description:** [Meta]

[Full blog post text]

**Word Count:** [N]  
**Target Keywords:** [List]

---

## 📝 YouTube — Short Script

**Hook (0-3s):** [Text]  
**Body (3-50s):** [Script]  
**CTA (50-60s):** [Text]

**Visual Notes:** [What to show on screen]

---

## 📅 Suggested Posting Schedule

| Day | Time | Platform | Content Piece | Status |
|-----|------|----------|---------------|--------|
| Mon | 09:00 | LinkedIn | Insight Post | [Approve / Edit / Reject] |
| Mon | 14:00 | X | Thread | [Approve / Edit / Reject] |
| Tue | 10:00 | Instagram | Carousel | [Approve / Edit / Reject] |
| ... | ... | ... | ... | ... |

---

## Quality Gates Passed
- [ ] AI Voice Humanizer
- [ ] Brand Safety Check
- [ ] Consistency Check
- [ ] Council Review (sample)
- [ ] Compliance Review (if [Your Brand]-related)

## Colin Approval
- [ ] Approve All
- [ ] Edit Selected
- [ ] Reject Selected
- [ ] Request Rewrite
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **AI Voice Humanizer** | Always | Every post sounds like Colin. No AI slop. No generic filler. Active voice. Sharp tone. |
| **Brand Safety Check** | Always | Promotional mentions are natural. No medical claims. No unsubstantiated guarantees. |
| **Consistency Check** | Always | All posts tell the same story. No contradictions. No verbatim repetition across platforms. |
| **Council Review** | Always (sample) | At least one post per platform reviewed by council. Must be on-brand and engaging. |
| **Compliance Review** | If [Your Brand]-related | No health or medical claims without substantiation. Reviewed by Compliance agent. |
| **Colin Approval** | Always | Colin approves each piece individually before scheduling. |
| **Length Check** | Always | Platform-appropriate lengths. LinkedIn: 150-350 words. X: 280 chars per tweet. Instagram: minimal text per slide. |

---

## Metrics to Track

### Per-Package Metrics (Tracked in Airtable)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Generation Time** | Source input → complete package ready | < 30 minutes |
| **Colin Approval Rate** | % of posts Colin approves without edits | > 60% |
| **Platform Coverage** | % of requested platforms generated | 100% |
| **Voice Accuracy** | Colin's rating: "Sounded like me" (1-5) | > 4.0 |
| **Council Pass Rate** | % of posts approved on first council review | > 70% |
| **Scheduled Posts** | % of approved posts that actually get published | > 80% |

### Platform-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Engagement Rate** | Likes + comments + shares / followers | Platform-specific benchmarks |
| **LinkedIn** | Avg. engagement per post | > 3% |
| **X** | Avg. engagement per tweet | > 2% |
| **Instagram** | Avg. engagement per post | > 4% |
| **Newsletter** | Open rate + click rate | Open > 30%, Click > 5% |
| **Blog** | Page views + time on page | Trending up |
| **YouTube** | Views + subscribers gained | Trending up |
| **Follower Growth** | Net new followers per week | Trending up |
| **CTA Conversion** | Clicks on links / CTAs | > 2% |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Content Leverage** | Total pieces generated / source pieces | 10:1 (one source → 10+ pieces) |
| **Posting Consistency** | % of days with at least one post | > 80% |
| **Angle Effectiveness** | Which angles (insight, story, contrarian) get the most engagement? | Reviewed monthly |
| **Format Effectiveness** | Which formats (carousel, thread, standalone) perform best per platform? | Reviewed monthly |
| **Promotional Balance** | % of posts that are purely value vs. promotional | 80/20 value/promotional |
| **Content Backlog** | Number of approved but unscheduled posts | < 5 (avoid backlog buildup) |

### Self-Learning Feedback Loop
- **Per-post:** When a post goes live, track engagement in the first 24h, 48h, 7d. Tag the angle, format, and platform. Feed into future generation.
- **Weekly:** Review top-performing posts. What did they have in common? (Angle? Format? Hook? Timing?) Update generation heuristics.
- **Weekly:** Review low-performing posts. What was missing? (Too long? Wrong angle? Poor timing?) Update generation heuristics.
- **Monthly:** Re-evaluate platform priorities. Is Instagram worth the effort? Should Colin focus more on LinkedIn and X? Adjust based on ROI.
- **Quarterly:** Re-evaluate content angles. Which angles consistently perform? Build more around them. Which angles never land? Deprecate.
- **Continuous:** Build a "content performance library" — every post tagged with angle, format, engagement, and outcome. Use top performers as few-shot examples for the agent.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **Trend Researcher** | This workflow | Trending topics + angles → content ideas to repurpose |
| **Analytics Agent** | This workflow | Performance data → identifies high-performing content worth amplifying |
| **Weekly Report** | This workflow | Content metrics from the week → input for the Monday report |
| This workflow | **Weekly Report** | Content output metrics (posts generated, scheduled, engagement) are aggregated into the Monday report |
| This workflow | **Proposal Writer** | High-performing content pieces become case studies and social proof in proposals |
| This workflow | **Outreach Sequencer** | Content links can be shared as value-adds in outreach sequences (e.g., "I wrote about this here...") |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

