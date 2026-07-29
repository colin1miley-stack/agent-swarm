# Proposal Writer Workflow

> **Purpose:** Transform a structured brief into a tailored, persuasive proposal that sounds like Colin wrote it — not a template, not AI slop.
> **Assigned Agent:** `Agent ProposalWriter` (or any agent with the `proposal-writing` skill + `ai-voice-humanizer` skill)
> **Category:** Conversion / Pre-Engagement

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Lead Brief Ready** | Lead Researcher workflow completes a brief; Colin clicks "Generate Proposal" | On demand |
| **Manual Request** | Colin drops a brief ID + pricing tier into the `#proposal-drafts` channel | On demand |
| **Batch Mode** | Colin provides a list of 5-10 brief IDs for batch proposal generation | On demand (flagged as batch) |
| **RFP Response** | Inbound RFP detected by Inbox Triage; brief is generated first, then proposal triggered | On demand |
| **Proposal Revision** | Colin marks an existing proposal as "needs revision" (e.g., pricing changed, scope expanded) | On demand |

**Auto-run threshold:** Never auto-runs. Always requires Colin or Council to explicitly trigger, due to high stakes and customization requirements.

---

## Input Format

### Required Fields
| Field | Description | Source |
|-------|-------------|--------|
| `brief_id` | Airtable record ID of the completed Lead Researcher brief | Lead Researcher output |
| `service_type` | Which service Colin is pitching | Colin's input (e.g., `ai-revenue-systems`, `vitae10-b2b`, `automation-audit`) |
| `pricing_tier` | Selected from Colin's standard tiers | Colin's input or auto-suggested from brief |

### Standard Pricing Tiers (Colin's Context)
| Tier | Price | Description | When to Use |
|------|-------|-------------|-------------|
| **Audit** | £500 | One-week deep dive into their current sales/marketing stack. Deliverable: findings + prioritized roadmap. | First engagement, low trust, proof of value |
| **System Build** | £2,500-5,000 | End-to-end implementation of one revenue system (e.g., AI outreach, content pipeline, lead scoring). | They have budget, clear problem, and trust |
| **Retainer** | £1,500-3,000/mo | Ongoing optimization, new system builds, and strategic advisory. | They want a partner, not a one-off |
| **Vitae10 B2B** | Custom | Corporate wellness program for their team. | Health-focused companies, HR benefits, team wellness |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `custom_scope` | Any deviations from the standard tier | `null` |
| `urgency_note` | "They need this by X date" or "They mentioned competitor Y" | `null` |
| `colin_tone` | `sharp`, `consultative`, `warm`, `aggressive` | `consultative` (default for most B2B) |
| `format` | `email-body`, `pdf-proposal`, `notion-page`, `loom-script` | `email-body` for cold outreach, `pdf-proposal` for warm leads |

**Input source:** Airtable record with brief + Colin's tier selection, or `#proposal-drafts` channel with explicit parameters.

---

## Step-by-Step Process

### Phase 1: Context Ingestion (Automated)
- [ ] **1.1 Load Brief**
  - Pull the full Lead Researcher brief from Airtable by `brief_id`
  - Verify brief is complete and not stale (> 30 days old = flag for re-research)
  - Extract: company snapshot, pain points, recommended angles, decision-maker profile, competitive context

- [ ] **1.2 Load Voice & Style Guide**
  - Pull Colin's voice profile from `IDENTITY.md` and `SOUL.md`
  - Pull recent approved proposals from the `proposals/` archive to pattern-match Colin's writing style
  - Identify the tone for this proposal based on `colin_tone` input and company culture signals

- [ ] **1.3 Load Pricing & Terms**
  - Pull the selected `pricing_tier` template
  - Apply any `custom_scope` modifications
  - Calculate total value, payment terms, and delivery timeline

### Phase 2: Customization & Drafting (Agent-Led)
- [ ] **2.1 Hook Construction**
  - Select the primary angle from the brief (or override with Colin's `urgency_note`)
  - Write a 2-3 sentence opener that: (a) demonstrates understanding of their specific pain, (b) hints at the solution without giving it all away, (c) feels like Colin wrote it after researching them, not a template
  - The hook should reference a specific detail from their company (recent post, hiring signal, tech gap) to prove this is custom

- [ ] **2.2 Problem-Solution Map**
  - Map each identified pain point to a specific capability Colin offers
  - Use their language, not Colin's. If they say "our sales process is manual and chaotic", mirror that back. If they say "we need to scale without hiring 10 people", use that phrase.
  - Quantify pain where possible: "If your team spends 10 hours/week on manual follow-up, that's 520 hours/year. At £50/hour, that's £26,000 in hidden cost."

- [ ] **2.3 Proof & Social Proof**
  - Include 1-2 relevant case studies or results from Colin's work
  - If no direct case study exists, use a parallel example (e.g., "A SaaS company in a similar growth stage saw X result")
  - Reference any mutual connections, shared communities, or public signals that build credibility

- [ ] **2.4 Scope & Deliverables**
  - Translate the selected pricing tier into specific deliverables using their context
  - Not: "AI automation setup" → Yes: "Automated lead qualification system that scores and routes inbound leads to the right rep within 60 seconds"
  - Include timeline: "Week 1: Audit. Week 2: Build. Week 3: Test. Week 4: Handoff."
  - Include what they need to provide (e.g., CRM access, 2 hours of stakeholder interviews)

- [ ] **2.5 Pricing Presentation**
  - Present price in context of value, not as a cost
  - Use anchoring: "Your team currently spends £X on manual work. This system eliminates that for £Y."
  - Include options: "If budget is a concern, we can start with the Audit at £500 and expand from there."
  - Payment terms: 50% upfront, 50% on delivery (or monthly for retainers)

- [ ] **2.6 Call to Action**
  - Specific, low-friction next step: "Reply to book a 20-min call this week" or "Here's a Calendly link — I have slots Tuesday and Thursday"
  - Avoid vague: "Let me know if you're interested" → Use: "Does Thursday at 2pm work for a quick call to walk through the scope?"
  - Include a fallback: "If this isn't the right timing, I can send a lighter audit option instead."

### Phase 3: Quality & Polish
- [ ] **3.1 AI Voice Humanizer Pass**
  - Run full draft through `ai-voice-humanizer` skill
  - Check for: robotic transitions, generic filler, passive voice, overused AI phrases ("leverage", "unlock", "delve", "In today's fast-paced world")
  - Replace with: specific details, active voice, conversational rhythm, Colin's natural vocabulary

- [ ] **3.2 Template Drift Check**
  - Agent does a diff against the base template: What % of the proposal is custom vs. template?
  - Target: > 70% custom text per proposal. If < 70%, flag for re-drafting.
  - Check: Does the proposal reference the company by name at least 5 times? Does it mention a specific pain point they actually have? Does it reference a real detail from their online presence?

- [ ] **3.3 Length Check**
  - Email body: 200-400 words (respect their time)
  - PDF proposal: 2-3 pages max (executive summary + scope + pricing + next steps)
  - Loom script: 2-3 minutes spoken (≈ 300-450 words)

- [ ] **3.4 Council Review**
  - Submit to `kimi-council` for critique
  - Council evaluates: Does this sound like Colin? Is the value clear? Is the ask specific? Would *you* reply to this?
  - Council can: approve, suggest 1-2 specific edits, or reject with a rewrite directive

- [ ] **3.5 Colin Delivery**
  - Post to `#proposal-drafts` with:
    - The full proposal text
    - A 3-bullet summary (what, why, next step)
    - The brief ID it was built from
    - The customization score (% custom text)
  - Colin reviews, edits, and approves before sending

---

## Output Format

### Email Body Format (Default)
```markdown
**Subject:** [Company-specific hook referencing their pain point or recent signal]

Hi [First Name],

[2-3 sentence opener that proves I've done my homework. Reference a specific detail from their recent post, hiring signal, or tech gap.]

[Their specific pain point, mirrored back in their language.] [Quantified cost or opportunity of not solving it.]

[What I do — framed as their outcome, not my process.] [Relevant proof point or parallel case study.]

[Specific scope, timeline, and deliverable description.] [Pricing, presented in context of value.]

[Specific CTA with time-bound options.] [Fallback option if they want to start smaller.]

[Sign-off in Colin's voice]
```

### PDF Proposal Format
```markdown
# Proposal: [Service Name] for [Company Name]

## Executive Summary
[3-4 sentences: What they need, why it matters, and what Colin will deliver.]

## The Problem
[2-3 paragraphs: Their specific pain points, quantified where possible, using their language.]

## The Solution
[2-3 paragraphs: What Colin will build, how it works, and what "done" looks like.]

## Scope & Deliverables
- [Deliverable 1] — [Description + timeline]
- [Deliverable 2] — [Description + timeline]
- [Deliverable 3] — [Description + timeline]

## Investment
**Total:** £[Amount]  
**Payment:** [Terms]  
**Timeline:** [Weeks]  
**ROI Context:** [How this pays for itself]

## What You Need to Provide
- [Requirement 1]
- [Requirement 2]

## Next Steps
[Specific action + timeline. Calendly link or proposed times.]

---
Colin Miley  
[Contact info]
```

### Metadata (Tracked in Airtable)
| Field | Value |
|-------|-------|
| `proposal_id` | Unique ID |
| `brief_id` | Parent brief |
| `service_type` | Which service |
| `pricing_tier` | Selected tier |
| `customization_score` | % custom text |
| `word_count` | Total length |
| `tone` | Selected tone |
| `council_approval` | Yes/No + notes |
| `colin_approval` | Yes/No + edits made |
| `sent_date` | When Colin sent it |
| `response` | Replied / No reply / Meeting booked |
| `deal_status` | Pending / Won / Lost / Stalled |

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Template Drift Check** | Always | > 70% custom text. Company name referenced ≥ 5 times. Specific pain points from brief are addressed. |
| **AI Voice Humanizer** | Always | No AI slop. Reads like Colin's natural voice. Active voice. Conversational. No filler. |
| **Council Review** | Always | 2+ council members. Must answer "Would I reply to this?" with yes or detailed no + fix. |
| **Colin Approval** | Always | Colin reads and approves before any proposal is sent. No exceptions. |
| **Length Check** | Always | Email: 200-400 words. PDF: 2-3 pages. Loom: 2-3 min script. |

---

## Metrics to Track

### Per-Proposal Metrics (Tracked in Airtable)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Customization Score** | % of text unique to this proposal vs. template | > 70% |
| **Draft-to-Approval Time** | First draft → Colin approval | < 30 minutes |
| **Colin Edit Rate** | % of proposals that require significant Colin edits | < 30% (if higher, voice training needs work) |
| **Reply Rate** | Proposals sent → replies received | > 25% for cold, > 50% for warm |
| **Meeting Booking Rate** | Proposals sent → meetings booked | > 15% for cold, > 40% for warm |
| **Win Rate** | Proposals sent → deals closed | > 10% for cold, > 30% for warm |
| **Average Deal Size** | £ per closed deal | Trending up over time |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Proposal Throughput** | Proposals drafted per week | > 5 (when Colin is actively selling) |
| **Voice Accuracy** | Colin's feedback: "This sounded like me" vs. "This needed heavy editing" | > 80% "sounded like me" |
| **Council Pass Rate** | % of proposals approved by council on first pass | > 70% |
| **Format Effectiveness** | A/B test: email vs. PDF vs. Loom — which converts better? | Reviewed monthly |

### Self-Learning Feedback Loop
- **Per-proposal:** Colin rates voice accuracy (1-5) and reply quality (1-5). Feed into humanizer training data.
- **Weekly:** Review which proposals got replies. What did they have in common? (Length? Specific detail? Pricing presentation?) Update drafting heuristics.
- **Monthly:** Audit lost proposals. Why did they lose? (Price? Timing? Scope mismatch?) Update pricing tiers or scope descriptions.
- **Quarterly:** Re-evaluate templates. Are any sections consistently getting edited by Colin? Those sections need better voice training or clearer guidance.
- **Continuous:** Build a "winning proposals" library. Tag proposals that led to deals. Use them as few-shot examples for future drafts.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **Lead Researcher** | This workflow | Brief is the primary input — all customization derives from it |
| **Content Repurposer** | This workflow | Case studies, testimonials, and proof points from content pipeline feed into proposals |
| This workflow | **Outreach Sequencer** | Approved proposal text can be sliced into sequence messages (e.g., day 1 = hook, day 3 = social proof, day 7 = case study) |
| This workflow | **Follow-up Chaser** | Proposal sent date + response status feeds the follow-up pipeline |
| This workflow | **Weekly Report** | Proposal metrics (sent, replied, won, lost) are aggregated into the Monday report |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

