# Weekly Report Workflow

> **Purpose:** Synthesize everything the agent swarm produced in the past week into a structured, actionable Monday morning report that gives Colin a clear picture of wins, risks, and next steps.
> **Assigned Agent:** `Agent WeeklyReport` (or any agent with the `reporting` skill + `ai-voice-humanizer` skill)
> **Category:** Operations / Strategic Rhythm

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Scheduled Run** | Every Monday at 08:00 GMT — auto-generates the weekly report | Weekly |
| **Manual Run** | Colin says "generate my weekly report" or "what happened last week?" | On demand |
| **End-of-Week** | Colin requests a "Friday wrap-up" version on Friday afternoon | On demand (Friday variant) |
| **Milestone Event** | Significant event occurs (deal closed, launch milestone, crisis) — Colin can request an ad-hoc report | On demand |

**Auto-run threshold:** Runs automatically every Monday at 08:00 GMT. No manual trigger required. Colin can also request it on-demand.

---

## Input Format

### Required Fields (Auto-Generated — No Colin Input Needed)
| Field | Description | Source |
|-------|-------------|--------|
| `report_period` | Start and end date of the week being reported | Auto: last Monday 00:00 → this Monday 00:00 |
| `report_type` | `standard` (full Monday report) or `friday-wrap` (lighter Friday version) | Auto: `standard` for Monday, `friday-wrap` if Friday |

### Data Sources (Auto-Aggregated)
| Data Source | What It Provides | How It's Accessed |
|-------------|-----------------|-------------------|
| **Lead Researcher** | Briefs generated, leads researched, quality scores | Airtable query |
| **Proposal Writer** | Proposals drafted, sent, replied, won, lost | Airtable query |
| **Outreach Sequencer** | Sequences sent, replies, meetings booked, angle performance | Airtable query |
| **Inbox Triage** | Emails processed, leads detected, urgent issues resolved | Daily log aggregation |
| **Follow-up Chaser** | Follow-ups sent, re-engagements, meetings recovered | Daily log aggregation |
| **Content Repurposer** | Content pieces generated, scheduled, engagement metrics | Airtable + Buffer analytics |
| **Analytics Agent** | Platform engagement, follower growth, CTA conversion | Buffer + Substack + LinkedIn analytics |
| **Competitor Monitoring** | Competitor moves, pricing changes, new launches | Weekly scan |
| **MEMORY.md** | Colin's current context, priorities, and recent decisions | File read |
| **Finance Tracker** | Weekly expense run rate, revenue, cash position | `finance/EXPENSE-TRACKER.md` |

---

## Step-by-Step Process

### Phase 1: Data Aggregation (Automated)
- [ ] **1.1 Pull Workflow Metrics**
  - Query each agent's Airtable/database for the past week's activity
  - Extract: volume, quality, outcomes, and notable events per workflow
  - Calculate week-over-week changes (e.g., +3 proposals vs. last week, -2 leads vs. last week)
  - Flag any workflow that produced zero output (is it broken? Is Colin not using it?)

- [ ] **1.2 Pull Engagement Metrics**
  - LinkedIn: Posts, engagement, follower growth, top-performing post
  - X: Tweets, engagement, follower growth, top-performing tweet
  - Instagram: Posts, engagement, follower growth
  - Newsletter: Send, open rate, click rate, replies
  - Blog: Page views, time on page, top post
  - YouTube: Videos, views, subscribers

- [ ] **1.3 Pull Financial Metrics**
  - Weekly revenue (deals closed, payments received)
  - Weekly expenses (from `finance/EXPENSE-TRACKER.md`)
  - Cash position (runway, burn rate if applicable)
  - Pipeline value (total £ value of open proposals)

- [ ] **1.4 Pull Context & Priorities**
  - Read `MEMORY.md` for Colin's current focus and recent decisions
  - Read recent `memory/YYYY-MM-DD.md` files for any daily notes
  - Check if any decisions from the previous week's report are still pending
  - Check for any upcoming deadlines or events in the next 7 days

### Phase 2: Analysis & Insight Generation
- [ ] **2.1 Win Identification**
  - Identify the top 3-5 wins of the week:
    - Deals closed (with £ value)
    - Meetings booked (with lead names)
    - Content pieces that outperformed expectations
    - Operational improvements (e.g., "Inbox Triage now auto-detects 95% of leads")
    - Personal wins (e.g., "Colin posted daily on LinkedIn for 7 days straight")
  - Quantify each win where possible ("£X in revenue", "Y% engagement increase")

- [ ] **2.2 Risk & Issue Identification**
  - Identify the top 3-5 risks or issues:
    - Stalled deals (value at risk)
    - Unresponsive suppliers or partners
    - Declining metrics (e.g., "LinkedIn engagement down 20% vs. last week")
    - Operational problems (e.g., "Inbox Triage missed 2 urgent emails")
    - Financial concerns (e.g., "Runway down to 3 months")
  - For each risk, suggest a specific action or flag for Colin's attention

- [ ] **2.3 Pattern Detection**
  - Look for patterns across workflows:
    - "3 proposals sent, 0 replies — possible pricing issue?"
    - "All inbound inquiries this week mentioned [competitor] — worth a competitive response?"
    - "Content engagement is highest on Tuesdays — should we shift posting schedule?"
    - "Supplier X has been non-responsive for 2 weeks — may need escalation"
  - Surface 1-2 non-obvious insights that Colin might miss by looking at individual workflows

- [ ] **2.4 Pipeline Health Check**
  - Total open leads: [N] (change from last week)
  - Total proposals sent: [N] (change from last week)
  - Total meetings booked: [N] (change from last week)
  - Total deals in negotiation: [N] (change from last week)
  - Pipeline velocity: How fast are leads moving from research → proposal → meeting → close?
  - Pipeline conversion: % of leads at each stage that advance to the next stage

- [ ] **2.5 Content Performance Check**
  - Total content pieces published: [N]
  - Total engagement (likes + comments + shares): [N]
  - Follower growth: [+N]
  - Top-performing piece: [Which one, on which platform, why it worked]
  - Bottom-performing piece: [Which one, possible reason]
  - Content-to-lead conversion: Did any content piece directly generate an inbound inquiry?

### Phase 3: Report Drafting
- [ ] **3.1 Structure the Report**
  - Use the template below (see Output Format)
  - Lead with the most important insight or win (not a generic "Here's your weekly report")
  - Keep it scannable: bullet points, bold headers, short paragraphs
  - Include specific numbers, names, and dates — not vague summaries

- [ ] **3.2 AI Voice Humanizer Pass**
  - Run the report through `ai-voice-humanizer`
  - Check for: robotic transitions, generic filler, passive voice, overly formal language
  - The report should feel like a sharp weekly brief from a trusted advisor, not a dashboard export

- [ ] **3.3 Self-Review**
  - Agent reads the report and asks:
    - "Would I want to read this on Monday morning?"
    - "Is anything missing that Colin would ask about?"
    - "Are the action items specific enough?"
    - "Did I surface any non-obvious insights?"
  - If the report is > 1,000 words, trim the least critical section

- [ ] **3.4 Quality Gate**
  - Submit to `kimi-council` for a quick review (optional for standard reports, required for milestone reports)
  - Council checks: Is the analysis sound? Are the recommendations actionable? Is anything obviously wrong?

### Phase 4: Delivery & Action Tracking
- [ ] **4.1 Colin Delivery**
  - Post report to `#weekly-reports` channel at 08:00 GMT Monday
  - Include a "Quick Scan" section at the top for Colin to read in 60 seconds
  - Include a "Deep Dive" section below for detailed review
  - For Friday wrap-ups: Post to `#weekly-reports` at 17:00 GMT Friday with a lighter format

- [ ] **4.2 Action Item Tracking**
  - Extract all action items from the report and add them to Colin's task system (Airtable or Notion)
  - Each action item should have: description, owner (Colin or agent), due date, and priority
  - Action items from the previous week's report should be checked for completion status

- [ ] **4.3 Archive & Feedback**
  - Save the report to `reports/weekly/YYYY-MM-DD.md`
  - If Colin provides feedback (e.g., "This was too long", "I wanted to see X"), log it in `MEMORY.md` for future report tuning

---

## Output Format

### Standard Monday Report

```markdown
# Weekly Report — [Week of Date] to [Date]
**Generated:** Monday, [Date] at 08:00 GMT  
**Report Type:** Standard Monday Brief  
**Colin's Current Focus:** [From MEMORY.md — e.g., "Launching Vitae10 concierge phase"]

---

## ⚡ Quick Scan (60 Seconds)

**Top Win:** [One sentence — e.g., "Closed £2,500 deal with Acme Corp + booked 3 new meetings"]

**Top Risk:** [One sentence — e.g., "Supplier X non-responsive for 10 days — may delay launch"]

**One Action for Today:** [One specific thing — e.g., "Call Supplier X directly before 12pm"]

**Mood Check:** [Up / Flat / Down — based on week-over-week metrics trend]

---

## 🏆 Wins This Week

### 1. [Win Title]
[What happened, why it matters, and the impact. Include £ value if applicable.]

### 2. [Win Title]
...

### 3. [Win Title]
...

---

## ⚠️ Risks & Issues

### 1. [Risk Title] — Priority: [High / Medium / Low]
[What's at risk, why it's happening, and what happens if unaddressed.]
**Suggested Action:** [Specific next step]
**Owner:** [Colin / Agent]
**Due:** [Date]

### 2. [Risk Title]
...

---

## 📊 Pipeline Health

| Metric | This Week | Last Week | Change | Trend |
|--------|-----------|-----------|--------|-------|
| Open Leads | [N] | [N] | [+/-N] | [↑/↓/→] |
| Proposals Sent | [N] | [N] | [+/-N] | [↑/↓/→] |
| Meetings Booked | [N] | [N] | [+/-N] | [↑/↓/→] |
| Deals Closed | [N] | [N] | [+/-N] | [↑/↓/→] |
| Pipeline Value | £[N] | £[N] | [+/-N] | [↑/↓/→] |
| Pipeline Velocity | [N days] | [N days] | [+/-N] | [↑/↓/→] |

**Key Observation:** [One insight about the pipeline — e.g., "Leads are moving faster from brief to proposal, but stalling at the meeting stage. Consider a stronger pre-meeting nurture sequence."]

---

## 📈 Content Performance

| Platform | Posts | Engagement | Follower Growth | Top Post |
|----------|-------|------------|-----------------|----------|
| LinkedIn | [N] | [N] | [+/-N] | [Brief description + link] |
| X | [N] | [N] | [+/-N] | [Brief description + link] |
| Instagram | [N] | [N] | [+/-N] | [Brief description + link] |
| Newsletter | [N] | [Open% / Click%] | [+/-N subs] | [Subject line] |
| Blog | [N] | [N views] | — | [Top post title] |

**Content Insight:** [One insight — e.g., "Contrarian takes outperformed practical advice 2:1 on LinkedIn this week. Double down on contrarian angles."]

---

## 💰 Financial Snapshot

| Metric | This Week | Last Week | Change |
|--------|-----------|-----------|--------|
| Revenue | £[N] | £[N] | [+/-N] |
| Expenses | £[N] | £[N] | [+/-N] |
| Net | £[N] | £[N] | [+/-N] |
| Cash Position | £[N] | £[N] | [+/-N] |
| Runway (months) | [N] | [N] | [+/-N] |

**Financial Note:** [Any notable expense, payment, or cash concern.]

---

## 🤖 Agent Swarm Performance

| Workflow | Output This Week | Quality Score | Notable Events |
|----------|-----------------|---------------|----------------|
| Lead Researcher | [N] briefs | [X/10] | [Any issues or wins] |
| Proposal Writer | [N] proposals | [X/10] | [Any issues or wins] |
| Outreach Sequencer | [N] sequences | [X/10] | [Any issues or wins] |
| Inbox Triage | [N] emails | [X/10] | [Any issues or wins] |
| Follow-up Chaser | [N] chases | [X/10] | [Any issues or wins] |
| Content Repurposer | [N] packages | [X/10] | [Any issues or wins] |

**Agent Insight:** [One insight — e.g., "Proposal Writer is getting faster (avg 20 min vs. 35 min last week) but Colin is editing more heavily. May need voice calibration."]

---

## 🔮 Pattern Detection

**Non-Obvious Insight #1:** [Something the agent noticed that Colin might not have — e.g., "Every meeting you booked this week came from a LinkedIn DM, not email. Consider shifting your outreach channel mix."]

**Non-Obvious Insight #2:** [Another pattern — e.g., "Your highest-engagement content this week was about your personal journey, not AI tactics. Your audience may be more interested in 'building in public' than tool tutorials."]

---

## 📋 Action Items from Last Week

| Action | Owner | Status | Notes |
|--------|-------|--------|-------|
| [Action 1] | [Colin/Agent] | [Done / In Progress / Not Started] | [Notes] |
| [Action 2] | [Colin/Agent] | [Done / In Progress / Not Started] | [Notes] |

---

## 📋 Action Items for This Week

| # | Action | Owner | Priority | Due |
|---|--------|-------|----------|-----|
| 1 | [Specific action] | [Colin/Agent] | [High/Medium/Low] | [Date] |
| 2 | [Specific action] | [Colin/Agent] | [High/Medium/Low] | [Date] |
| 3 | [Specific action] | [Colin/Agent] | [High/Medium/Low] | [Date] |

---

## 🗓️ Upcoming This Week

| Day | Event | Notes |
|-----|-------|-------|
| Mon | [Event] | [Notes] |
| Tue | [Event] | [Notes] |
| Wed | [Event] | [Notes] |
| Thu | [Event] | [Notes] |
| Fri | [Event] | [Notes] |

---

## 📝 Agent Notes

[Any observations, suggestions, or context the agent wants to share with Colin that don't fit the sections above.]
```

### Friday Wrap-Up (Lighter Variant)

```markdown
# Friday Wrap-Up — [Date]
**Quick Wins:** [2-3 things that went well this week]
**Stuck Items:** [2-3 things that need attention before Monday]
**Weekend Note:** [Anything Colin should be aware of over the weekend]
**Monday Preview:** [What's scheduled or queued for Monday]

**One Thing to Feel Good About:** [A positive note to end the week]
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Data Accuracy** | Always | All numbers, names, and dates are verified against source data. No hallucinated metrics. |
| **Insight Quality** | Always | At least 2 non-obvious insights per report. Generic observations are rejected. |
| **AI Voice Humanizer** | Always | Report reads like a sharp advisor's brief, not a dashboard export. No filler. |
| **Actionability** | Always | Every risk has a suggested action. Every action item has an owner and due date. |
| **Length Check** | Always | Standard report: 800-1,200 words. Quick Scan section: readable in 60 seconds. |
| **Council Review** | For milestone reports | 2+ council members review for accuracy and insight quality. |

---

## Metrics to Track

### Per-Report Metrics (Tracked in Airtable)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Generation Time** | Data aggregation → report delivery | < 30 minutes |
| **Colin Engagement** | Does Colin read the report? (Reaction, reply, or action taken) | > 90% |
| **Action Item Completion** | % of action items from the report that are completed by the next report | > 70% |
| **Insight Value** | Colin's feedback: "This insight was useful" (1-5) | > 4.0 |
| **Accuracy Score** | Colin's feedback: "Numbers were correct" (1-5) | > 4.5 |
| **Length Appropriateness** | Colin's feedback: "Length was right" (1-5) | > 4.0 |

### System-Level Metrics (Tracked Monthly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Report Reliability** | % of reports delivered on time (Monday 08:00) | 100% |
| **Trend Detection** | How many weeks in advance did the report flag a trend before it became obvious? | > 1 week |
| **Decision Support** | How many decisions did Colin make based on the report? | > 3 per week |
| **Workflow Optimization** | How many workflow improvements were suggested and implemented based on report insights? | > 1 per month |

### Self-Learning Feedback Loop
- **Per-report:** Colin provides feedback on report quality (reaction, reply, or explicit feedback). Log in `MEMORY.md`.
- **Weekly:** Review which sections Colin engages with most (reads, replies, acts on). Expand those sections. Deprecate ignored sections.
- **Weekly:** Review which action items from last week's report were completed. If < 70%, investigate why (too many items? unclear ownership? wrong priorities?).
- **Monthly:** Re-evaluate report structure. Is the Quick Scan section working? Should there be a new section (e.g., "Competitor Moves")? Update template.
- **Quarterly:** Review historical reports. Are there trends the agent missed? Is the analysis getting better over time? Update analysis methodology.
- **Continuous:** Build a "report performance library" — each report tagged with Colin's feedback, action item completion, and any decisions made. Use to tune future reports.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **All Workflows** | This workflow | Every workflow's metrics and outputs are aggregated into the weekly report |
| **Analytics Agent** | This workflow | Engagement and performance data feeds into the Content Performance and Agent Swarm sections |
| **Competitor Monitoring** | This workflow | Competitor moves feed into the Risks & Issues section |
| **Finance Tracker** | This workflow | Revenue, expenses, and cash position feed into the Financial Snapshot |
| This workflow | **All Workflows** | Action items and recommendations from the report flow back to the relevant workflows for execution |
| This workflow | **MEMORY.md** | Key insights and decisions are logged in MEMORY.md for long-term context |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

