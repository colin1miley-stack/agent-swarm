# WORKFLOWS_INDEX.md — Agent Swarm Master Map

> **Purpose:** The single source of truth for how Colin's 7-agent swarm operates. Every workflow, every trigger, every handoff, every dependency — mapped in one place.  
> **Last Updated:** 2026-07-01  
> **Owner:** Agent Swarm Architecture (maintain this file as workflows evolve)

---

## The Swarm at a Glance

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        COLIN'S AGENT SWARM — DATA FLOW                       │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌─────────────────┐
    │  EXTERNAL INPUT │
    │  (leads, emails,│
    │  content ideas, │
    │  market signals)│
    └────────┬────────┘
             │
    ┌────────┴────────┐
    │   INGESTION       │
    │   ┌───────────┐   │
    │   │  Lead     │   │  ←── URL/name/LinkedIn → structured brief
    │   │  Researcher│   │
    │   └─────┬─────┘   │
    │   ┌───────────┐   │
    │   │  Inbox    │   │  ←── Unread emails → sorted, prioritized, drafted
    │   │  Triage   │   │
    │   └─────┬─────┘   │
    │   ┌───────────┐   │
    │   │  Content  │   │  ←── One idea → 10+ platform-specific posts
    │   │ Repurposer│   │
    │   └─────┬─────┘   │
    └─────────┬─────────┘
              │
    ┌─────────┴─────────┐
    │    ACTIVATION       │
    │   ┌───────────┐     │
    │   │  Proposal │     │  ←── Brief + pricing → tailored proposal
    │   │  Writer   │     │
    │   └─────┬─────┘     │
    │   ┌───────────┐     │
    │   │  Outreach │     │  ←── Brief + voice → multi-touch sequence
    │   │ Sequencer │     │
    │   └─────┬─────┘     │
    │   ┌───────────┐     │
    │   │  Follow-up│     │  ←── Open threads → prioritized chase list
    │   │  Chaser   │     │
    │   └─────┬─────┘     │
    └─────────┬───────────┘
              │
    ┌─────────┴─────────┐
    │   SYNTHESIS       │
    │   ┌───────────┐   │
    │   │  Weekly   │   │  ←── All outputs → Monday morning report
    │   │  Report   │   │
    │   └─────┬─────┘   │
    └─────────┬─────────┘
              │
    ┌─────────┴─────────┐
    │    FEEDBACK LOOP    │
    │   (wins → skills →  │
    │    better agents)   │
    └─────────────────────┘
```

---

## Workflow Registry

| # | Workflow | Agent | Purpose | Trigger Frequency | File |
|---|----------|-------|---------|-------------------|------|
| 01 | **Lead Researcher** | `Agent LeadResearcher` | Turn raw lead into structured brief | On demand + weekly scan + auto-triggers | [`01-lead-researcher.md`](01-lead-researcher.md) |
| 02 | **Proposal Writer** | `Agent ProposalWriter` | Turn brief into tailored proposal | On demand (Colin-approved only) | [`02-proposal-writer.md`](02-proposal-writer.md) |
| 03 | **Outreach Sequencer** | `Agent OutreachSequencer` | Turn brief into multi-touch sequence | On demand (Colin-approved only) | [`03-outreach-sequencer.md`](03-outreach-sequencer.md) |
| 04 | **Inbox Triage** | `Agent InboxTriage` | Read, sort, draft replies for all emails | Every 2 hours (business hours) + on demand | [`04-inbox-triage.md`](04-inbox-triage.md) |
| 05 | **Follow-up Chaser** | `Agent FollowUpChaser` | Monitor open threads, prioritize, draft chases | Daily (09:00 GMT) + on demand | [`05-follow-up-chaser.md`](05-follow-up-chaser.md) |
| 06 | **Content Repurposer** | `Agent ContentRepurposer` | Turn one idea into 10+ platform posts | Weekly + on demand + trend triggers | [`06-content-repurposer.md`](06-content-repurposer.md) |
| 07 | **Weekly Report** | `Agent WeeklyReport` | Synthesize all outputs into Monday brief | Every Monday 08:00 GMT + on demand | [`07-weekly-report.md`](07-weekly-report.md) |

---

## Trigger Matrix

### When Does Each Workflow Run?

| Trigger Type | Lead Researcher | Proposal Writer | Outreach Sequencer | Inbox Triage | Follow-up Chaser | Content Repurposer | Weekly Report |
|--------------|-----------------|-----------------|--------------------|--------------|------------------|--------------------|---------------|
| **Scheduled** | Weekly scan (Mon) | — | — | Every 2h (09-18) | Daily 09:00 | Weekly sprint (Mon) | Mon 08:00 |
| **Manual** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Auto-Trigger** | ✅ (URL + 30d stale) | ❌ (Colin must approve) | ❌ (Colin must approve) | ✅ (volume >10) | ✅ (stalled >7d) | ✅ (backlog >7d) | ✅ (auto) |
| **Upstream Signal** | Inbox → flag lead, Competitor → detect target | Lead Brief → ready | Proposal → approved, Brief → ready | External inbox | Inbox → flag, Sequencer → complete, Proposal → sent | Trend Researcher → topic, Analytics → high performer | All workflows → metrics |
| **Event-Based** | Post-event attendee list | Post-event RFP | Post-event follow-up | — | Post-event | Post-event transcript | Milestone events |

### Trigger Hierarchy

```
HOT LEAD (Colin says "go now")
    → Lead Researcher (if not already briefed)
    → Proposal Writer (if Colin wants proposal)
    → Outreach Sequencer (if Colin wants outreach)
    → Follow-up Chaser (monitors for replies)

INBOUND EMAIL
    → Inbox Triage (every 2h)
    → Lead Researcher (if new lead detected)
    → Proposal Writer (if RFP/inquiry)
    → Outreach Sequencer (if nurture needed)
    → Follow-up Chaser (if reply requires follow-up)

CONTENT IDEA
    → Content Repurposer (weekly + on demand)
    → Weekly Report (metrics aggregated)

MONDAY MORNING
    → Weekly Report (auto-runs 08:00)
    → Content Repurposer (weekly sprint)
    → Lead Researcher (weekly network scan)
```

---

## Interdependency Map

### Data Flow Diagram

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                              INTERDEPENDENCY MAP                                │
└────────────────────────────────────────────────────────────────────────────────┘

[External Sources]
    │
    ├──→ Lead Researcher ←──┐
    │                        │
    ├──→ Inbox Triage ──────┤
    │    │                   │
    │    ↓                   │
    │    Lead Researcher     │
    │    │                   │
    │    ↓                   │
    │    Proposal Writer ←───┤
    │    │                   │
    │    ↓                   │
    │    Outreach Sequencer ←┤
    │    │                   │
    │    ↓                   │
    │    Follow-up Chaser ←──┤
    │    │                   │
    │    ↓                   │
    │    Weekly Report ←─────┤
    │                        │
    ├──→ Content Repurposer ←┘
    │    │
    │    ↓
    │    Weekly Report
    │
    └──→ Weekly Report (financial data, competitor data, etc.)
```

### Detailed Handoffs

| From | To | What Flows | Trigger Condition |
|------|----|-----------|-------------------|
| **Inbox Triage** | **Lead Researcher** | Inbound inquiry flagged as lead | When a new company/person contacts Colin and no brief exists |
| **Competitor Monitoring** | **Lead Researcher** | Competitor growth/hiring signals | When competitor activity suggests new target customers |
| **Lead Researcher** | **Proposal Writer** | Completed brief | When Colin selects a pricing tier and clicks "Generate Proposal" |
| **Lead Researcher** | **Outreach Sequencer** | Brief (angles, pain points, decision-maker profile) | When Colin approves an outreach sequence for a briefed lead |
| **Lead Researcher** | **Follow-up Chaser** | Decision-maker contact details + engagement history | When a lead enters the pipeline and needs monitoring |
| **Proposal Writer** | **Outreach Sequencer** | Approved proposal text (sliced into sequence messages) | When a proposal is approved but Colin wants a nurture sequence first |
| **Proposal Writer** | **Follow-up Chaser** | Proposal sent date + response status | When a proposal is marked "sent" → auto-adds to follow-up pipeline |
| **Proposal Writer** | **Weekly Report** | Proposal metrics (drafted, sent, replied, won, lost) | Aggregated weekly into the Monday report |
| **Outreach Sequencer** | **Inbox Triage** | Replies to sequences | When a reply comes in, Inbox Triage routes it appropriately |
| **Outreach Sequencer** | **Follow-up Chaser** | Completed sequences with no reply | When day 14 passes with no response → adds to follow-up pipeline |
| **Outreach Sequencer** | **Weekly Report** | Sequence metrics (sent, replied, booked) | Aggregated weekly into the Monday report |
| **Inbox Triage** | **Proposal Writer** | Inbound RFP or formal inquiry | After brief generation, if the inquiry is formal enough for a proposal |
| **Inbox Triage** | **Follow-up Chaser** | Emails flagged as "needs follow-up in X days" | When a reply requires a delayed follow-up |
| **Inbox Triage** | **Weekly Report** | Triage metrics (processed, detected, resolved) | Aggregated weekly into the Monday report |
| **Follow-up Chaser** | **Outreach Sequencer** | Re-engaged threads that need a new sequence | When a stalled lead replies positively but doesn't commit |
| **Follow-up Chaser** | **Weekly Report** | Follow-up metrics (chases, re-engagements, meetings) | Aggregated weekly into the Monday report |
| **Content Repurposer** | **Weekly Report** | Content metrics (generated, scheduled, engagement) | Aggregated weekly into the Monday report |
| **Content Repurposer** | **Proposal Writer** | High-performing content pieces become case studies | When a piece demonstrates strong results → use as social proof |
| **Content Repurposer** | **Outreach Sequencer** | Content links as value-adds in sequences | When a relevant piece exists → include as a share in outreach |
| **Trend Researcher** | **Content Repurposer** | Trending topics + angles → content ideas | When a trend aligns with Colin's expertise |
| **Analytics Agent** | **Content Repurposer** | High-performing content → amplify | When a piece outperforms → create more like it |
| **Analytics Agent** | **Weekly Report** | Engagement and performance data | Weekly aggregation into the report |
| **Competitor Monitoring** | **Weekly Report** | Competitor moves, pricing changes, launches | Weekly aggregation into the report |
| **Finance Tracker** | **Weekly Report** | Revenue, expenses, cash position | Weekly aggregation into the report |
| **Weekly Report** | **All Workflows** | Action items and recommendations | When a report identifies a workflow improvement → feeds back to that workflow |
| **Weekly Report** | **MEMORY.md** | Key insights and decisions | When a report surfaces something worth remembering long-term |
| **All Workflows** | **Weekly Report** | All metrics and outputs | Weekly auto-aggregation |

---

## The Self-Learning Loop

This is not a static system. Every workflow tracks its own metrics and feeds wins back into skill improvement.

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                           SELF-LEARNING ARCHITECTURE                            │
└────────────────────────────────────────────────────────────────────────────────┘

     ┌─────────────┐
     │   WORKFLOW  │
     │   EXECUTES  │
     └──────┬──────┘
            │
            ↓
     ┌─────────────┐
     │   METRICS   │
     │   TRACKED   │
     │  (Airtable) │
     └──────┬──────┘
            │
            ↓
     ┌─────────────┐
     │   WEEKLY    │
     │   REVIEW    │
     │  (Monday)   │
     └──────┬──────┘
            │
            ↓
     ┌─────────────┐     ┌─────────────┐
     │  PATTERN    │────→│   SKILL     │
     │  DETECTED?  │     │  IMPROVEMENT│
     └──────┬──────┘     └─────────────┘
            │                     │
            ↓                     │
     ┌─────────────┐              │
     │   NO →      │              │
     │  Continue   │              │
     │  Tracking   │              │
     └─────────────┘              │
                                  │
            ↓                     │
     ┌─────────────┐              │
     │   YES →     │──────────────┘
     │  Update     │
     │  Workflow   │
     │  Heuristics │
     └─────────────┘
            │
            ↓
     ┌─────────────┐
     │  UPDATE     │
     │  SKILL.md   │
     │  (GitHub)   │
     └─────────────┘
            │
            ↓
     ┌─────────────┐
     │  NEXT RUN   │
     │  USES NEW   │
     │  HEURISTICS │
     └─────────────┘
```

### Feedback Loops by Workflow

| Workflow | Metric Captured | Feedback Loop | Skill Update |
|----------|-----------------|-------------|------------|
| **Lead Researcher** | Brief accuracy, angle correctness, meeting conversion | Review which briefs led to meetings → update pain point derivation checklist | Update `lead-research` skill heuristics |
| **Proposal Writer** | Reply rate, win rate, Colin edit rate, customization score | Review winning proposals → update templates and voice training | Update `proposal-writing` skill + `ai-voice-humanizer` training data |
| **Outreach Sequencer** | Reply rate by touch, channel, angle, cadence | Review winning sequences → update angle progression and cadence | Update `outreach-sequence` skill heuristics |
| **Inbox Triage** | Classification accuracy, draft quality, urgent detection rate | Review misclassified emails → update classification rules and urgency keywords | Update `email-automation` skill rules |
| **Follow-up Chaser** | Re-engagement rate, meeting recovery, chase fatigue | Review which chases got replies → update scoring model and strategy selection | Update `follow-up` skill scoring weights |
| **Content Repurposer** | Engagement by platform, angle, format, promotional balance | Review top performers → update angle priorities and format recommendations | Update `content-repurposing` skill + `linkedin-post-writing` heuristics |
| **Weekly Report** | Colin engagement, action completion, insight value | Review which insights were useful → update analysis methodology | Update `reporting` skill template and heuristics |

---

## Quality Gates Across the Swarm

Every workflow has mandatory quality gates. Here's the full matrix:

| Gate | Lead Researcher | Proposal Writer | Outreach Sequencer | Inbox Triage | Follow-up Chaser | Content Repurposer | Weekly Report |
|------|-----------------|-----------------|--------------------|--------------|------------------|--------------------|---------------|
| **AI Voice Humanizer** | ✅ | ✅ | ✅ | ✅ (drafts) | ✅ | ✅ | ✅ |
| **Council Review** | ✅ (if hot) | ✅ | ✅ | — | ✅ | ✅ (sample) | ✅ (milestones) |
| **Colin Approval** | ✅ (if hot) | ✅ | ✅ | ✅ (before sending) | ✅ | ✅ | — (auto) |
| **Self-Review** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Template/Drift Check** | — | ✅ (>70% custom) | ✅ (per message) | — | — | ✅ (consistency) | — |
| **Length Check** | — | ✅ (200-400w / 2-3pp) | ✅ (platform limits) | — | — | ✅ (platform limits) | ✅ (<1,200w) |
| **Data Accuracy** | ✅ (source URLs) | ✅ (pricing correct) | ✅ (details from brief) | ✅ (no hallucinations) | ✅ (thread context) | — | ✅ (all metrics verified) |
| **Brand Safety** | — | — | — | — | — | ✅ (no med claims) | — |
| **Compliance Review** | — | — | — | — | — | ✅ (if Vitae10) | — |

**Legend:** ✅ = Always required, ✅ (condition) = Required under specific conditions, — = Not applicable

---

## Airtable Schema (Shared Data Layer)

All workflows share a single Airtable base. Here's the unified schema:

### Table: `Leads`
| Field | Type | Used By |
|-------|------|---------|
| `lead_id` | Primary ID | All workflows |
| `company_name` | Text | Lead Researcher, Proposal Writer, Outreach Sequencer |
| `company_url` | URL | Lead Researcher |
| `industry` | Select | Lead Researcher, Proposal Writer |
| `headcount` | Number | Lead Researcher |
| `stage` | Select (cold/warm/proposal_sent/meeting_held/negotiation/closed_won/closed_lost) | Follow-up Chaser, Weekly Report |
| `priority` | Select (hot/warm/cold) | Lead Researcher, Follow-up Chaser |
| `brief_id` | Link → Briefs | Lead Researcher |
| `proposal_id` | Link → Proposals | Proposal Writer |
| `sequence_id` | Link → Sequences | Outreach Sequencer |
| `deal_value` | Currency | Follow-up Chaser, Weekly Report |
| `last_contact_date` | Date | Follow-up Chaser |
| `next_action_date` | Date | Follow-up Chaser, Inbox Triage |
| `source` | Select | Lead Researcher |
| `assigned_agent` | Text | All workflows |
| `created_at` | Date | All workflows |
| `updated_at` | Date | All workflows |

### Table: `Briefs`
| Field | Type | Used By |
|-------|------|---------|
| `brief_id` | Primary ID | Lead Researcher |
| `lead_id` | Link → Leads | Lead Researcher |
| `pain_points` | Long Text | Proposal Writer, Outreach Sequencer |
| `recommended_angles` | Long Text | Outreach Sequencer |
| `decision_maker` | JSON | Outreach Sequencer, Follow-up Chaser |
| `customization_score` | Percent | Lead Researcher |
| `confidence` | Select | Lead Researcher |
| `generated_at` | Date | Lead Researcher |
| `colin_approval` | Select | Lead Researcher |

### Table: `Proposals`
| Field | Type | Used By |
|-------|------|---------|
| `proposal_id` | Primary ID | Proposal Writer |
| `brief_id` | Link → Briefs | Proposal Writer |
| `lead_id` | Link → Leads | Proposal Writer |
| `service_type` | Select | Proposal Writer |
| `pricing_tier` | Select | Proposal Writer |
| `customization_score` | Percent | Proposal Writer |
| `status` | Select (draft/approved/sent/replied/won/lost) | Proposal Writer, Follow-up Chaser, Weekly Report |
| `sent_date` | Date | Follow-up Chaser, Weekly Report |
| `response` | Select | Proposal Writer, Weekly Report |
| `word_count` | Number | Proposal Writer |
| `council_approval` | Select | Proposal Writer |
| `colin_approval` | Select | Proposal Writer |

### Table: `Sequences`
| Field | Type | Used By |
|-------|------|---------|
| `sequence_id` | Primary ID | Outreach Sequencer |
| `lead_id` | Link → Leads | Outreach Sequencer |
| `brief_id` | Link → Briefs | Outreach Sequencer |
| `channel_mix` | Select | Outreach Sequencer |
| `goal` | Select | Outreach Sequencer |
| `touch_count` | Number | Outreach Sequencer |
| `reply_touch` | Number | Outreach Sequencer |
| `status` | Select (draft/approved/sent/completed) | Outreach Sequencer, Follow-up Chaser |
| `personalization_score` | Number | Outreach Sequencer |
| `council_approval` | Select | Outreach Sequencer |
| `colin_approval` | Select | Outreach Sequencer |

### Table: `EmailThreads`
| Field | Type | Used By |
|-------|------|---------|
| `thread_id` | Primary ID | Inbox Triage, Follow-up Chaser |
| `lead_id` | Link → Leads | Inbox Triage, Follow-up Chaser |
| `sender` | Email | Inbox Triage |
| `subject` | Text | Inbox Triage |
| `priority` | Select | Inbox Triage |
| `category` | Select | Inbox Triage |
| `draft_reply` | Long Text | Inbox Triage |
| `draft_confidence` | Select | Inbox Triage |
| `status` | Select | Inbox Triage, Follow-up Chaser |
| `last_message_date` | Date | Follow-up Chaser |
| `follow_up_due` | Date | Follow-up Chaser |

### Table: `ContentPieces`
| Field | Type | Used By |
|-------|------|---------|
| `piece_id` | Primary ID | Content Repurposer |
| `source_id` | Text | Content Repurposer |
| `platform` | Select | Content Repurposer |
| `format` | Select | Content Repurposer |
| `angle` | Text | Content Repurposer |
| `content` | Long Text | Content Repurposer |
| `status` | Select (draft/approved/scheduled/published) | Content Repurposer |
| `scheduled_date` | Date | Content Repurposer |
| `engagement` | Number | Content Repurposer, Weekly Report |
| `colin_approval` | Select | Content Repurposer |

### Table: `Reports`
| Field | Type | Used By |
|-------|------|---------|
| `report_id` | Primary ID | Weekly Report |
| `week_start` | Date | Weekly Report |
| `week_end` | Date | Weekly Report |
| `report_type` | Select | Weekly Report |
| `file_path` | URL | Weekly Report |
| `colin_feedback` | Select | Weekly Report |
| `action_items_completed` | Number | Weekly Report |
| `action_items_total` | Number | Weekly Report |

---

## Shared Skills Stack

These skills are used across multiple workflows:

| Skill | Used By | Purpose |
|-------|---------|---------|
| `ai-voice-humanizer` | All 7 workflows | De-AI polish, ensure Colin's voice |
| `kimi-council` | Lead Researcher, Proposal Writer, Outreach Sequencer, Content Repurposer, Weekly Report | Critical review, quality gate |
| `defuddle` | Lead Researcher, Content Repurposer | Clean web content extraction |
| `email-automation` | Inbox Triage, Follow-up Chaser | Email reading, sending, tracking |
| `linkedin-post-writing` | Content Repurposer | LinkedIn-specific formatting |
| `content-repurposing` | Content Repurposer | Multi-platform adaptation |
| `proposal-writing` | Proposal Writer | Proposal structure and customization |
| `outreach-sequence` | Outreach Sequencer | Sequence design and cadence |
| `follow-up` | Follow-up Chaser | Scoring and strategy selection |
| `reporting` | Weekly Report | Data aggregation and synthesis |
| `lead-research` | Lead Researcher | Research methodology |
| `analytics-tracking` | Content Repurposer, Weekly Report | Engagement metrics |
| `competitor-monitoring` | Lead Researcher, Weekly Report | Competitor signals |

---

## Onboarding a New Workflow

When adding an 8th (or 9th, or 10th) workflow to the swarm:

1. **Create the workflow file** in `agent-swarm/workflows/XX-workflow-name.md`
2. **Update this index** — add to the registry, trigger matrix, interdependency map, quality gate matrix, and Airtable schema
3. **Define interdependencies** — which workflows feed into it? Which does it feed into?
4. **Add metrics** — what does this workflow track? How does it feed the self-learning loop?
5. **Add quality gates** — which gates are required? Which are optional?
6. **Update the Weekly Report** — does this workflow produce metrics that should be included?
7. **Test the handoff** — run a dry run to ensure data flows correctly between upstream and downstream workflows

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial swarm architecture — 7 workflows, full interdependency map, self-learning loop |

---

*This is a living document. Update it whenever workflows change, new agents are added, or the data schema evolves.*
