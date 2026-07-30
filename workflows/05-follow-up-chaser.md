# Follow-up Chaser Workflow

> **Purpose:** Ensure no conversation dies from neglect. Monitor all open threads, prioritize who needs a follow-up, draft the message, and queue it for Colin's approval. Never let a warm lead go cold.
> **Assigned Agent:** `Agent FollowUpChaser` (or any agent with the `follow-up` skill + `ai-voice-humanizer` skill)
> **Category:** Operations / Active Pipeline

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Scheduled Run** | Daily at 09:00 GMT — morning sweep of all open threads | Daily |
| **Manual Run** | Colin says "who needs a follow-up?" or "chase my pipeline" | On demand |
| **Event Trigger** | Inbox Triage flags an email as "needs follow-up in X days" | Real-time |
| **Stalled Pipeline Alert** | Any thread with no activity for 7+ days | Weekly scan |
| **Post-Meeting** | Colin marks a meeting as "completed, needs follow-up" in Airtable | On demand |
| **Proposal Sent** | Proposal Writer marks a proposal as "sent" → auto-adds to follow-up pipeline | On demand |
| **Sequence Completed** | Outreach Sequencer completes day 14 with no reply → auto-adds to follow-up pipeline | On demand |

---

## Input Format

### Required Fields
| Field | Description | Source |
|-------|-------------|--------|
| `pipeline_source` | Which pipeline(s) to scan: `all`, `sales`, `partnerships`, `suppliers`, `media` | Default: `all` |
| `lookback_days` | How far back to check for stalled threads | Default: 7 |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `max_threads` | Max threads to process in one run | 30 |
| `priority_filter` | Only show threads matching this priority | `null` (all priorities) |
| `colin_context` | What Colin is currently focused on | From `MEMORY.md` |
| `tone_override` | `casual`, `professional`, `warm`, `urgent` | `null` (auto-detected from thread history) |
| `custom_message` | Colin wants to add a specific note to the follow-up | `null` |

---

## Step-by-Step Process

### Phase 1: Pipeline Scanning
- [ ] **1.1 Identify Open Threads**
  - Scan all tracked communication threads across:
    - Airtable "Sales Pipeline" (proposals sent, meetings held, negotiations)
    - Airtable "Partnerships" (operations discussions, co-marketing, integrations)
    - Email threads where Colin was the last sender and no reply received
    - LinkedIn DM threads where Colin sent the last message and no reply
    - Outreach sequences that completed with no reply
  - Define "open thread":
    - Last message from Colin (or agent on Colin's behalf)
    - No reply received within `lookback_days`
    - Thread not explicitly marked as "lost" or "closed" in Airtable

- [ ] **1.2 Thread Metadata Extraction**
  - For each open thread, extract:
    - `lead_name`, `company`, `last_contact_date`, `last_message_type` (email/DM/proposal/meeting), `thread_age` (days since last contact)
    - `value_estimate`: Potential deal value (from Airtable or inferred from service tier)
    - `stage`: `proposal_sent`, `meeting_held`, `negotiation`, `nurture`, `cold_outreach`, `supplier_discussion`
    - `previous_follow_ups`: How many times has this thread been followed up already?
    - `reply_history`: Has this person ever replied before? (Yes = warm, No = cold)
    - `context_notes`: Any notes from Colin or other agents about this thread

### Phase 2: Prioritization
- [ ] **2.1 Scoring Model**
  - Each thread gets a "Chase Score" (0-100) based on:
    - **Value** (0-30): Higher deal value = higher score. £5K+ = 30, £1K-5K = 20, <£1K = 10
    - **Recency** (0-25): More recent = lower score (less urgent). 3 days = 5, 7 days = 15, 14 days = 25
    - **Stage** (0-20): Later stage = higher score. `negotiation` = 20, `meeting_held` = 15, `proposal_sent` = 12, `cold_outreach` = 5
    - **Warmth** (0-15): Has replied before = 15, engaged on LinkedIn = 10, cold = 5
    - **Context** (0-10): Matches `colin_context` (e.g., if Colin is focusing on [Your Brand] launch, supplier threads get +10)

- [ ] **2.2 Priority Buckets**
  - **Priority A (Score 70-100):** Chase today. These are high-value, late-stage, stalled threads. Colin must see these.
  - **Priority B (Score 40-69):** Chase this week. Important but not emergency. Queue for Colin's review.
  - **Priority C (Score 20-39):** Gentle nudge. Low-value or early-stage. Consider if worth chasing at all.
  - **Priority D (Score < 20):** Let it go. Either too cold, too low-value, or too recent. Mark for review in 30 days.

- [ ] **2.3 Contextual Adjustments**
  - If a thread is a operations discussion and Colin's context is "[Your Brand] launch", boost by 10 points
  - If a thread is a partnership inquiry and Colin's context is "content growth", boost by 10 points
  - If a thread has been followed up 3+ times already, cap score at 50 (don't chase forever)
  - If a thread is marked "lost" in Airtable, exclude from scoring (unless Colin manually overrides)

### Phase 3: Follow-up Message Drafting
- [ ] **3.1 Context Retrieval**
  - For each Priority A and B thread, pull the full thread history
  - Pull the Lead Researcher brief (if available) for personalization data
  - Check `MEMORY.md` for any recent notes about this lead or topic
  - Check if there are any new signals (e.g., the company posted about a pain point on LinkedIn since the last contact)

- [ ] **3.2 Message Strategy Selection**
  - Select the follow-up strategy based on `stage` and `previous_follow_ups`:

  | Stage | Follow-up # | Strategy | Tone |
  |-------|-------------|----------|------|
  | `cold_outreach` | 1 | Soft bump: "Just checking this landed" | Casual |
  | `cold_outreach` | 2 | New angle: "Also saw you posted about X" | Warm |
  | `cold_outreach` | 3+ | Close loop: "Totally understand if timing is off" | Professional |
  | `proposal_sent` | 1 | Gentle nudge: "Quick question on the proposal" | Professional |
  | `proposal_sent` | 2 | Add value: "Here's a case study that might help" | Warm |
  | `proposal_sent` | 3+ | Direct ask: "Are you still considering this?" | Sharp |
  | `meeting_held` | 1 | Recap + next step: "As discussed, here's the summary" | Professional |
  | `meeting_held` | 2 | Timeline check: "Wanted to check on timing" | Warm |
  | `negotiation` | 1 | Value reinforcement: "Happy to address any questions" | Professional |
  | `negotiation` | 2 | Urgency (soft): "Just flagging this before month-end" | Sharp |
  | `supplier_discussion` | 1 | Status check: "Where are we on the timeline?" | Professional |
  | `supplier_discussion` | 2 | Escalation prep: "Need to confirm by X date for launch" | Urgent |

- [ ] **3.3 Message Drafting**
  - Draft a follow-up message that:
    - References the last interaction specifically (not "following up on my email")
    - Adds new value (insight, resource, case study, or relevant update) — never just "bumping"
    - Includes a soft or direct ask appropriate to the stage
    - Feels natural, not robotic — like Colin remembered to check in, not like a system nudged him
  - If `custom_message` provided by Colin, weave it into the draft naturally

- [ ] **3.4 AI Voice Humanizer Pass**
  - Run each draft through `ai-voice-humanizer`
  - Check for: passive voice, generic follow-up language, overly apologetic tone, robotic transitions
  - Ensure the message feels like Colin remembered to send it, not like a CRM auto-reminder

### Phase 4: Output Assembly & Colin Review
- [ ] **4.1 Build Follow-up Report**
  - Compile the prioritized follow-up list (see Output Format)
  - Include: thread summary, last contact, stage, deal value, chase score, and draft message

- [ ] **4.2 Quality Gate**
  - Self-review: Does every Priority A thread have a draft? Are any scores clearly wrong?
  - Check for duplicates: Is this thread already being chased by Outreach Sequencer or another agent?

- [ ] **4.3 Colin Delivery**
  - Post follow-up report to `#follow-up-chaser` channel
  - Priority A threads: @mention Colin with one-liner summary
  - Colin can: approve and send, edit the draft, skip this thread, or mark as "lost"

- [ ] **4.4 Action Execution**
  - If Colin approves, the agent sends the follow-up via the appropriate channel (email, LinkedIn DM, etc.)
  - If Colin edits, the agent sends the edited version
  - If Colin marks as "lost", the agent updates Airtable and stops chasing
  - If Colin skips, the agent reschedules for the next scheduled run

---

## Output Format

```markdown
# Follow-up Chaser Report — [Date] [Time] GMT
**Pipeline Scanned:** [sales / partnerships / suppliers / all]  
**Lookback:** [N] days  
**Threads Found:** [N] open  
**Prioritized for Chase:** [N]  
**Chase Time:** [X minutes]

---

## 🔴 Priority A — Chase Today (Score 70-100)

### Thread 1: [Lead Name] @ [Company] — Score: [XX]
**Stage:** [proposal_sent / meeting_held / etc.]  
**Last Contact:** [Date] ([N] days ago)  
**Channel:** [Email / LinkedIn DM / X]  
**Deal Value:** £[Amount]  
**Previous Follow-ups:** [N]  
**Has Replied Before:** [Yes / No]

**Thread Summary:**
[2-3 sentence recap of the conversation history and where it stands]

**Draft Follow-up:**
> [Draft message in Colin's voice]

**Why Chase Now:** [Specific reason — e.g., "Proposal sent 7 days ago, no reply, £3K deal"]
**Suggested Send Time:** [e.g., "Tuesday 10:00 AM for visibility"]

**Colin Action:** [ ] Approve & Send  [ ] Edit  [ ] Skip  [ ] Mark as Lost

---

### Thread 2: ...

## 🟠 Priority B — Chase This Week (Score 40-69)

### Thread 1: ...
[Same format as Priority A]

## 🟡 Priority C — Gentle Nudge (Score 20-39)

### Thread 1: ...
[Summary + draft, but Colin can choose to skip without guilt]

## ⚪ Priority D — Let It Go (Score < 20)

[List of threads with brief note: why they're low priority. Colin can override if desired.]

---

## Quick Stats
- **Priority A:** [N] threads
- **Priority B:** [N] threads
- **Priority C:** [N] threads
- **Priority D:** [N] threads
- **Total Pipeline Value at Risk:** £[Sum of deal values for A+B threads]
- **Oldest Stalled Thread:** [Lead] — [N] days
- **Most Follow-ups on One Thread:** [Lead] — [N] times

## Agent Notes
- [Any patterns, observations, or anomalies]
- [e.g., "Three proposal threads stalled at the same stage — possible pricing objection?"]
- [e.g., "Supplier X hasn't replied in 10 days — may need Colin to call directly"]
- [e.g., "One warm lead replied 'budget review next month' — flag for follow-up in 4 weeks"]
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Scoring Accuracy** | Always | Priority A threads must be genuinely urgent. No low-value cold leads in Priority A. |
| **Duplicate Detection** | Always | Check that a thread isn't already being actively chased by Outreach Sequencer or another workflow. |
| **AI Voice Humanizer** | Always (for draft messages) | Follow-ups sound like Colin. No "just following up" or "circling back" — add value every time. |
| **Context Awareness** | Always | Messages must reference the specific thread history, not be generic. |
| **Colin Approval** | Always | No follow-up is sent without Colin's approval. Drafts are presented; Colin decides. |

---

## Metrics to Track

### Per-Run Metrics (Tracked in Daily Log)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Scan Speed** | Total threads scanned / time taken | < 30 seconds per thread |
| **Prioritization Accuracy** | Colin's feedback: "Was this priority correct?" | > 90% |
| **Draft Quality** | Colin's edits: How many words changed per draft? | < 20% edit rate |
| **Re-engagement Rate** | Follow-ups sent → replies received | > 30% for Priority A, > 20% for Priority B |
| **Meeting Booking Rate** | Follow-ups sent → meetings booked | > 10% for Priority A |
| **Lost-to-Won Recovery** | Follow-ups on "lost" threads that eventually convert | > 5% |
| **Colin Approval Rate** | % of drafts Colin approves without edits | > 60% |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|---------|
| **Pipeline Staleness** | % of open threads with no activity in 7+ days | < 30% |
| **Follow-up Coverage** | % of stalled threads that get a follow-up within 7 days of stalling | > 90% |
| **Chase Fatigue** | Avg. number of follow-ups per thread before close/loss | < 4 (if higher, scoring or strategy needs tuning) |
| **Value at Risk** | Total £ value of stalled threads not yet chased | Minimized; review weekly |
| **Win Rate by Stage** | % of threads at each stage that eventually close | Track and optimize for underperforming stages |
| **Channel Effectiveness** | Reply rate by follow-up channel (email vs. LinkedIn vs. X) | Reviewed monthly |

### Self-Learning Feedback Loop
- **Per-follow-up:** When a reply comes in, tag which priority, stage, and strategy got it. Feed into the scoring model.
- **Weekly:** Review which follow-ups got replies. What did they have in common? (Value-add? Specific timing? Channel?) Update strategy selection.
- **Weekly:** Review which follow-ups were ignored. What was missing? (Too generic? Wrong timing? Already chased too many times?) Update scoring and strategy.
- **Monthly:** Re-evaluate the scoring weights. Is the value component too heavy? Is the recency component too light? Adjust based on what actually converts.
- **Quarterly:** Build a "winning follow-ups" library. Tag follow-ups that led to re-engagement and eventual deals. Use as few-shot examples.
- **Continuous:** When a thread is marked "lost", capture the reason. Is there a common objection or pattern? Create a new follow-up variant that addresses it earlier.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **Inbox Triage** | This workflow | Replies flagged as "needs follow-up in X days" → added to follow-up pipeline |
| **Outreach Sequencer** | This workflow | Completed sequences with no reply → added to follow-up pipeline |
| **Proposal Writer** | This workflow | Proposals marked as "sent" → added to follow-up pipeline |
| This workflow | **Outreach Sequencer** | Re-engaged threads may need a new outreach sequence if they reply positively but don't commit |
| This workflow | **Weekly Report** | Follow-up metrics (chases sent, re-engagements, meetings booked) are aggregated into the Monday report |
| This workflow | **Lead Researcher** | If a thread is re-engaged after a long stall, brief may be stale → trigger re-research before next follow-up |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

