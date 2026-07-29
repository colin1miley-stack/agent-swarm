# Outreach Sequencer Workflow

> **Purpose:** Generate a multi-touch, multi-channel outreach sequence (DMs + emails) that converts a lead from cold to warm, using Colin's voice and the intelligence from the brief.
> **Assigned Agent:** `Agent OutreachSequencer` (or any agent with the `outreach-sequence` skill + `ai-voice-humanizer` skill)
> **Category:** Engagement / Active Outreach

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Proposal Approved** | Colin approves a proposal; sequence is auto-generated to deliver the proposal over multiple touches | On demand |
| **Brief-Only Outreach** | Colin wants to reach out before a formal proposal (e.g., LinkedIn DM first, warm up, then email) | On demand |
| **Event Follow-up** | Post-event: Colin uploads a list of contacts met; sequence is generated for each | Post-event |
| **Re-engagement** | Follow-up Chaser identifies a stalled prospect; sequence is generated to re-ignite | On demand |
| **Inbound Warm-Up** | Inbound inquiry comes in but isn't ready for a proposal; sequence nurtures them to readiness | On demand |

**Auto-run threshold:** Never auto-runs. Colin explicitly approves the sequence before any messages are sent. The agent can draft and queue, but sending requires human approval for each message.

---

## Input Format

### Required Fields
| Field | Description | Source |
|-------|-------------|--------|
| `brief_id` | Lead Researcher brief record ID | Brief output |
| `channel_mix` | Which channels to use: `linkedin-only`, `email-only`, `linkedin+email`, `x-only`, `multi` | Colin's input or auto-suggested from lead signals |
| `sequence_goal` | `book-meeting`, `get-reply`, `deliver-proposal`, `nurture` | Colin's input |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `proposal_id` | If a proposal exists, link it | `null` |
| `tone` | `sharp`, `consultative`, `warm`, `casual` | `consultative` |
| `personalization_depth` | `light` (name + company), `medium` (pain point + angle), `deep` (specific posts + mutuals + custom hook) | `medium` |
| `avoid_topics` | Anything to avoid mentioning | `null` |
| `known_objection` | If Colin knows a specific objection (e.g., "they're already using HubSpot") | `null` |
| `mutual_connection` | Name of a mutual connection for social proof | `null` |

---

## Step-by-Step Process

### Phase 1: Sequence Design (Agent-Led)
- [ ] **1.1 Channel Selection**
  - Based on `channel_mix` and available contact data:
    - LinkedIn: Use if decision-maker's LinkedIn is known and their profile is active (posts in last 30 days)
    - Email: Use if email address is known or inferable (from website, brief, or previous contact)
    - X/Twitter: Use if they are active on X and Colin is following them (or they follow Colin)
  - Default priority: LinkedIn DM first (lower friction) → Email if no response → X as last resort

- [ ] **1.2 Cadence Design**
  - Standard cadence (default for `book-meeting` goal):
    - **Day 1:** Initial touch — value-first, no ask, prove you've done homework
    - **Day 3:** Follow-up — reference their recent activity or add a relevant insight
    - **Day 7:** Social proof or case study — "Here's how someone like you solved this"
    - **Day 14:** Final touch — direct ask, soft close, or "permission to close the loop"
  - Short cadence (for `get-reply` goal):
    - **Day 1:** Direct ask + value
    - **Day 3:** Bounce with new angle
    - **Day 7:** Final touch
  - Nurture cadence (for `nurture` goal):
    - **Day 1:** Value share (article, insight, tool)
    - **Day 7:** Value share #2 (different angle)
    - **Day 14:** Soft ask
    - **Day 30:** Value share #3

- [ ] **1.3 Angle Progression**
  - Map the brief's top 3 angles across the sequence:
    - Day 1: Angle 1 (primary pain point)
    - Day 3: Angle 2 (different pain point or complementary angle)
    - Day 7: Angle 3 (social proof / case study angle)
    - Day 14: Direct ask (revisit angle 1 with urgency)
  - If a `known_objection` exists, address it in day 3 or day 7

### Phase 2: Message Drafting (Per-Touch)

#### Day 1: The Hook (No Ask)
- [ ] **2.1 Message Construction**
  - Lead with a specific observation about THEM, not about Colin
  - Reference a recent post, hiring signal, or company milestone from the brief
  - Add one insight or resource that demonstrates expertise without selling
  - No ask. No CTA. Just value and proof of homework.
  - Example: "Saw your team just opened a RevOps role — that's usually a signal that lead volume is outpacing manual process. Have you looked at automated qualification workflows? I wrote a short guide on how one SaaS company cut their response time from 4 hours to 60 seconds. Happy to share if useful."

- [ ] **2.2 Length Check**
  - LinkedIn DM: 50-100 words (mobile-friendly, scannable)
  - Email: 100-150 words (one short paragraph + one insight)
  - X DM: 30-50 words (extremely concise)

#### Day 3: The Follow-Up (Add Value)
- [ ] **2.3 Message Construction**
  - Reference their reply (if any) or their recent activity since day 1
  - Add a new angle, insight, or piece of social proof
  - Soft ask appears here: "Worth a quick chat?" or "Does this resonate with what you're seeing?"
  - Example: "Also noticed you're hiring for SDRs — that's another signal. The companies I've seen scale fastest didn't hire more SDRs; they made each SDR 3x more productive with the right stack. Here's a 2-min breakdown: [loom link]."

#### Day 7: The Social Proof (Case Study)
- [ ] **2.4 Message Construction**
  - Lead with a specific, relevant case study or result
  - Mirror their situation: "You mentioned [pain point]. A [similar company] had the same issue and [result]."
  - Direct ask: "Does Thursday 2pm work for a 15-min call to see if this applies to your setup?"
  - Include a specific time option (reduces friction)

#### Day 14: The Close (Final Touch)
- [ ] **2.5 Message Construction**
  - Acknowledge the non-response without being passive-aggressive
  - Restate the value in one sentence
  - Soft close: "If this isn't the right time, no worries — I'll close the loop. But if you want to grab 15 mins, I have slots Tuesday and Thursday."
  - Or: Offer a lower-friction next step (e.g., "If a call is too much, I can send a 2-min Loom instead")

### Phase 3: Quality & Assembly
- [ ] **3.1 AI Voice Humanizer Pass**
  - Run each message through `ai-voice-humanizer`
  - Check for: AI-sounding phrases, passive voice, generic openings, overly formal language
  - Ensure each message sounds like Colin sent it from his phone, not his desk

- [ ] **3.2 Personalization Depth Check**
  - `light`: Name + company mentioned. Is it there?
  - `medium`: Specific pain point + angle from brief. Is it referenced?
  - `deep`: Specific post/activity + mutual connection + custom hook. Is it all there?
  - Flag if the requested depth is not achievable with available data

- [ ] **3.3 Sequence Coherence Check**
  - Read all 4 messages in order. Do they tell a story? Do they escalate naturally?
  - Check for repetition: Are the same phrases used across messages? (Eliminate.)
  - Check for contradictions: Does day 3 contradict day 1? (Fix.)
  - Check for tone consistency: Does the sequence feel like one person, one voice?

- [ ] **3.4 Council Review**
  - Submit full sequence to `kimi-council`
  - Council evaluates: Would you reply to any of these? Which one? Why or why not?
  - Council can: approve, suggest edits to specific messages, or reject with rewrite directive

- [ ] **3.5 Colin Approval & Queue**
  - Present sequence in `#outreach-sequences` with:
    - Each message shown in its channel format (LinkedIn DM style, email style, etc.)
    - Send dates and times
    - A 1-sentence summary of the angle progression
    - A "send" button per message (Colin approves each one individually)
  - Colin can: approve all, edit any message, delay or skip any touch, or reject the sequence

---

## Output Format

```markdown
# Outreach Sequence: [Lead Name] @ [Company]
**Goal:** [book-meeting / get-reply / deliver-proposal / nurture]  
**Channels:** [LinkedIn + Email / LinkedIn only / etc.]  
**Personalization Depth:** [light / medium / deep]  
**Brief ID:** [ID]  
**Sequence ID:** [Unique ID]

---

## Touch 1 — Day 1 ([Date]) — [Channel]
**Status:** [Draft / Approved / Sent / Replied / No Reply]

[Message text, formatted for the channel]

**Angle:** [Primary angle from brief]  
**Personalization:** [What makes this specific to them]  
**Expected Response:** [What we're hoping for]

---

## Touch 2 — Day 3 ([Date]) — [Channel]
**Status:** [Draft / Approved / Sent / Replied / No Reply]

[Message text]

**Angle:** [Secondary angle]  
**New Value Add:** [What new insight or proof is introduced]  
**Soft Ask:** [What we're asking for]

---

## Touch 3 — Day 7 ([Date]) — [Channel]
**Status:** [Draft / Approved / Sent / Replied / No Reply]

[Message text]

**Angle:** [Social proof / case study angle]  
**Direct Ask:** [Specific meeting request or next step]  
**Time Options:** [Proposed times]

---

## Touch 4 — Day 14 ([Date]) — [Channel]
**Status:** [Draft / Approved / Sent / Replied / No Reply]

[Message text]

**Angle:** [Final angle / urgency / soft close]  
**Fallback:** [Lower-friction alternative if they don't want a call]  
**Close Reason:** [Why we're closing the loop if no response]

---

## Sequence Analytics
- **Total Touches:** 4
- **Estimated Send Window:** [Day 1 date] to [Day 14 date]
- **Channel Mix:** [LinkedIn: 3, Email: 1]
- **Personalization Score:** [X/10]
- **Colin Approval:** [Yes / No + edits]
- **Council Approval:** [Yes / No + notes]
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Personalization Check** | Always | Each message must reference a specific detail about the lead. Generic messages are rejected. |
| **AI Voice Humanizer** | Always | Each message sounds like Colin. No "I hope this email finds you well." No AI filler. |
| **Sequence Coherence** | Always | Messages tell a story, escalate naturally, don't contradict each other. |
| **Council Review** | Always | 2+ council members. At least one must say "I'd reply to this." |
| **Colin Approval** | Always | Colin approves each message individually before it's queued for sending. |
| **Length Check** | Always | LinkedIn: 50-100 words. Email: 100-150 words. X: 30-50 words. |

---

## Metrics to Track

### Per-Sequence Metrics (Tracked in Airtable)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Reply Rate** | Sequences sent → any reply received | > 25% for cold, > 50% for warm |
| **Meeting Booking Rate** | Sequences sent → meetings booked | > 15% for cold, > 40% for warm |
| **Reply-by-Touch** | Which touch got the reply? | Day 1: 40%, Day 3: 30%, Day 7: 20%, Day 14: 10% |
| **Personalization Score** | Agent-rated 1-10 on how custom the sequence is | > 7/10 |
| **Sequence Completion Rate** | % of sequences where all 4 touches are sent (not abandoned) | > 80% |
| **Colin Edit Rate** | % of messages Colin edits before sending | < 30% |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Sequence Throughput** | Sequences drafted per week | > 5 (when actively selling) |
| **Channel Effectiveness** | Reply rate by channel (LinkedIn vs Email vs X) | Reviewed monthly to optimize channel mix |
| **Angle Effectiveness** | Which angles from the brief get the most replies? | Reviewed monthly; top angles become default priorities |
| **Cadence Effectiveness** | Does 4-touch (day 1/3/7/14) outperform 3-touch (day 1/7/14)? | A/B test quarterly |
| **Nurture vs Direct** | Do nurture sequences (value-first) outperform direct-ask sequences? | A/B test quarterly |

### Self-Learning Feedback Loop
- **Per-sequence:** When a reply comes in, tag which touch got it and what the reply said. Feed into future sequence design.
- **Weekly:** Review sequences that got replies. What did they have in common? (Angle? Length? Channel? Specific detail?) Update drafting heuristics.
- **Weekly:** Review sequences that got no reply. What was missing? (Too generic? Wrong angle? Wrong channel? Bad timing?) Update triggers.
- **Monthly:** Re-evaluate cadence. Is 4-touch too many? Should day 3 be day 4? Test and measure.
- **Quarterly:** Build a library of "winning sequences" — sequences that led to meetings. Use them as few-shot examples.
- **Continuous:** When a lead says "no", capture the reason. Is it a common objection? Create a new sequence variant that addresses it upfront.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **Lead Researcher** | This workflow | Brief provides angles, pain points, and personalization data |
| **Proposal Writer** | This workflow | Approved proposal can be sliced into sequence messages (e.g., day 7 = scope summary, day 14 = direct proposal link) |
| **Inbox Triage** | This workflow | Replies from sequences are triaged and routed back to this workflow (e.g., "Interested — let's talk" triggers meeting booking) |
| This workflow | **Follow-up Chaser** | Sequences that get no reply flow into the follow-up pipeline after day 14 |
| This workflow | **Weekly Report** | Sequence metrics (sent, replied, booked) are aggregated into the Monday report |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

