# Metrics Framework — What to Track Per Workflow

> Every agent has a scorecard. Metrics are pulled automatically from tools, not entered manually. The goal: know if an agent is worth its compute cost.

## Lead Researcher Agent

**Purpose:** Find high-fit prospects, enrich with data, produce briefs.

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Briefs produced / week** | Agent logs | ≥ 10 | Count entries in `outputs.jsonl` where agent=lead_researcher |
| **Brief quality score** | Colin's feedback (1-5) | ≥ 4 | After each brief, Colin replies with 👍 (5), 👍👍 (4), 😐 (3), 👎 (2), or 🗑️ (1) |
| **Conversion to outreach** | Airtable/Notion | ≥ 30% | % of briefs that become outreach sequences (track status change) |
| **Accuracy (bad leads)** | Colin's feedback | ≤ 10% | % of briefs flagged as "wrong company / wrong person / outdated info" |
| **Time saved vs manual** | Estimate | ≥ 2 hrs/brief | Colin estimates time to do same research manually; agent logs estimate |

**Scorecard formula:**
```
Lead Researcher ROI = (briefs × quality_score × time_saved) / (compute_cost + colin_review_time)
```

**Red flag:** Quality score < 3 for 3+ briefs in a row → investigate skill, brief template, or data source.

## Proposal Writer Agent

**Purpose:** Draft proposals, quotes, and pitch documents for prospects.

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Proposals drafted / week** | Agent logs | ≥ 3 | Count `outputs.jsonl` entries |
| **Acceptance rate** | Airtable / manual | ≥ 20% | % of proposals that convert to won deals |
| **Time-to-close (with vs without agent)** | Airtable | Faster with agent | Compare cycle time for deals with agent-drafted proposals vs. without |
| **Feedback sentiment** | Colin's feedback | Positive | After each proposal, Colin notes: "use as-is", "minor edits", "major rewrite", or "discard" |
| **Colin edit time** | Estimate | ≤ 10 min/proposal | Track how long Colin spends editing vs. writing from scratch |

**Scorecard formula:**
```
Proposal Writer ROI = (accepted_proposals × deal_value × time_saved) / (compute_cost + colin_edit_time)
```

**Red flag:** "Discard" or "major rewrite" for 3+ proposals in a row → review proposal template and skill.

## Outreach Sequencer Agent

**Purpose:** Generate and send outreach sequences (email, LinkedIn, etc.).

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Sequences sent / week** | Agent logs | ≥ 5 | Count `outputs.jsonl` entries |
| **Open rate** | Klaviyo / email API | ≥ 40% | Track per sequence variant |
| **Reply rate** | Klaviyo / email API | ≥ 8% | Track per sequence variant |
| **Meeting booking rate** | Calendly / Airtable | ≥ 2% | % of sequences that result in a booked meeting |
| **Unsubscribe rate** | Klaviyo | ≤ 1% | Track to ensure not spammy |
| **Colin's time per sequence** | Estimate | ≤ 5 min | Colin reviews before send; track time spent |

**Scorecard formula:**
```
Outreach Sequencer ROI = (meetings_booked × pipeline_value × time_saved) / (compute_cost + colin_review_time + tool_cost)
```

**Red flag:** Reply rate < 2% for 10+ sequences → immediate skill review. Subject line, personalization, or CTA is broken.

## Inbox Triage Agent

**Purpose:** Sort, prioritize, draft replies, and surface action items from Colin's inbox.

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Emails triaged / day** | Agent logs | ≥ 50 | Count entries in `outputs.jsonl` |
| **Accuracy of sorting** | Colin's feedback | ≥ 95% | Colin corrects mis-sorted emails; agent logs corrections |
| **Reply quality (drafted replies)** | Colin's feedback | ≥ 80% use-as-is | % of drafted replies sent without edit or with minor edit |
| **Time saved / day** | Estimate | ≥ 30 min | Colin estimates time saved vs. manual inbox management |
| **Missed urgent emails** | Colin's feedback | 0 | Any urgent email not flagged by agent = critical failure |

**Scorecard formula:**
```
Inbox Triage ROI = (emails_triaged × time_saved × accuracy) / (compute_cost + colin_correction_time)
```

**Red flag:** Missed urgent email → immediate review of priority rules. Accuracy < 90% → review classification skill.

## Follow-up Chaser Agent

**Purpose:** Track open threads, send follow-ups, and close stale conversations.

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Threads tracked** | Agent logs | ≥ 20 active | Count active threads in tracking system |
| **Re-engagement rate** | Agent logs / email API | ≥ 15% | % of follow-ups that get a reply |
| **Thread closure rate** | Agent logs | ≥ 30% | % of stale threads that get a definitive close (reply, decline, or auto-close) |
| **Colin's time / week** | Estimate | ≤ 10 min | Colin reviews follow-up list weekly; track time |
| **Over-follow-up complaints** | Colin's feedback | 0 | Any "stop emailing me" = skill failure |

**Scorecard formula:**
```
Follow-up Chaser ROI = (re_engagements × pipeline_value + threads_closed × time_saved) / (compute_cost + colin_review_time)
```

**Red flag:** Over-follow-up complaint → immediate disable. Re-engagement rate < 5% → review follow-up timing and tone.

## Content Repurposer Agent

**Purpose:** Turn one piece of content into platform-optimized posts (LinkedIn, X, newsletter, etc.).

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Content pieces repurposed / week** | Agent logs | ≥ 3 | Count `outputs.jsonl` entries |
| **Engagement rate (LinkedIn)** | Buffer / LinkedIn API | ≥ 2% | (likes + comments + shares) / impressions |
| **Engagement rate (X/Twitter)** | Buffer / X API | ≥ 1% | Same formula |
| **Save rate (LinkedIn)** | LinkedIn API | ≥ 0.5% | Saves / impressions |
| **Share rate (LinkedIn)** | LinkedIn API | ≥ 0.3% | Shares / impressions |
| **Newsletter open rate** | Substack / Klaviyo | ≥ 30% | Track per issue |
| **Newsletter click rate** | Substack / Klaviyo | ≥ 5% | Track per issue |
| **Colin's time / piece** | Estimate | ≤ 5 min | Track editing time per repurposed piece |

**Scorecard formula (per platform):**
```
Content Repurposer ROI = (engagement × follower_growth × time_saved) / (compute_cost + colin_edit_time + tool_cost)
```

**Red flag:** Engagement rate < 0.5% for 3+ posts → review hook, format, or platform fit. Newsletter open rate < 15% → review subject line skill.

## Weekly Report Agent

**Purpose:** Summarize what happened this week, what needs attention, and what to do next.

| Metric | Source | Target | How to Track |
|--------|--------|--------|--------------|
| **Reports produced / week** | Agent logs | 1 | Count `outputs.jsonl` entries |
| **Action rate** | Colin's feedback | ≥ 60% | % of reports where Colin takes ≥ 1 action based on the report |
| **Accuracy (missed items)** | Colin's feedback | ≤ 10% | Any important item Colin notices that the report missed |
| **Colin's time / report** | Estimate | ≤ 5 min | Track reading + action time |
| **Colin's satisfaction** | Colin's feedback (1-5) | ≥ 4 | After each report, Colin rates usefulness |

**Scorecard formula:**
```
Weekly Report ROI = (actions_taken × impact_score × time_saved) / (compute_cost + colin_read_time)
```

**Red flag:** Action rate < 30% for 3+ weeks → report is noise, not signal. Investigate what's being reported vs. what Colin actually cares about.

## Universal Metrics (All Agents)

| Metric | Source | Target | Why It Matters |
|--------|--------|--------|---------------|
| **Compute cost / agent / month** | API billing | ≤ £20 | Ensure agent is worth its cost |
| **Colin's review time / agent / month** | Estimate | ≤ 30 min | Total manual time per agent |
| **Error rate** | Agent logs / Colin's feedback | ≤ 5% | % of outputs with factual errors, broken links, or bad formatting |
| **Cycle time (request → output)** | Agent logs | ≤ 5 min | How long the agent takes to produce output |
| **Colin's override rate** | Colin's feedback | ≤ 20% | % of outputs where Colin makes significant changes |

## ROI Threshold: Should This Agent Exist?

An agent should be kept if:

```
(Annual time saved × Colin's hourly value) ≥ (Annual compute cost + Annual tool cost + Annual Colin review time × Colin's hourly value)
```

**Colin's hourly value:** Use £100/hour (conservative for a founder's time).

**Example:**
- Outreach Sequencer saves 3 hours/week = 156 hours/year = £15,600 value
- Compute cost: £20/month × 12 = £240/year
- Tool cost (Klaviyo): £50/month × 12 = £600/year
- Colin review time: 20 min/month × 12 = 4 hours/year = £400 value
- **Net ROI: £15,600 - (£240 + £600 + £400) = £14,360/year**
- **Verdict: KEEP**

**Cut threshold:** If net ROI < £1,000/year, consider cutting the agent or merging it into another.

## Data Collection Automation

### Daily (Automated)
- Pull email metrics from Klaviyo/Outlook API
- Pull social metrics from Buffer API
- Pull Shopify metrics from Shopify API
- Count outputs from `outputs.jsonl`

### Weekly (5-minute manual)
- Colin rates brief quality, proposal feedback, report satisfaction
- Quick emoji reactions: 👍 = good, 😐 = meh, 👎 = bad
- Note any missed items, errors, or complaints

### Monthly (Automated + 30-min review)
- Agent compiles all metrics into scorecards
- Agent calculates ROI per agent
- Colin reviews scorecards and decides: keep / improve / cut / merge

## File Structure

```
agent-swarm/learning/metrics/
├── 2026-07/
│   ├── lead_researcher.json     ← Weekly scorecard snapshots
│   ├── proposal_writer.json
│   ├── outreach_sequencer.json
│   ├── inbox_triage.json
│   ├── follow_up_chaser.json
│   ├── content_repurposer.json
│   ├── weekly_report.json
│   └── universal.json           ← Compute cost, error rate, etc.
└── 2026-08/
    └── ...
```

## Quick Reference: What Colin Needs to Do

| Frequency | Action | Time |
|-----------|--------|------|
| Daily | Nothing (all automated) | 0 min |
| Weekly | Emoji reactions + quick notes on 3-5 outputs | 5 min |
| Monthly | Review scorecards, approve skill changes | 30 min |

**Total time investment: ~35 min/month.**
