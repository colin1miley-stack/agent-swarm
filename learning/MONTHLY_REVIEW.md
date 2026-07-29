# Monthly Review — The Cadence

> One structured review per month. 30 minutes max. Data pulled automatically. Decisions made quickly. Skills updated only when there's clear evidence.

## When

**Last Friday of every month, 10:00 AM.**

Why Friday? Gives the weekend to digest. Monday is fresh start with new skills active.

If Colin is traveling or swamped, the review can be postponed by up to 1 week. But it doesn't get skipped — it's the engine of the learning system.

## Before the Review (Automated, 0 Minutes of Colin's Time)

The agent prepares everything by Thursday evening:

### 1. Pull Data (Automated)

| Data Source | What to Pull | How |
|-------------|--------------|-----|
| **Notion** | Task completion rates, action items from weekly reports, project status | Notion API |
| **Buffer** | Social engagement rates, best-performing posts, follower growth | Buffer API |
| **Klaviyo** | Email open/reply/click rates, sequence performance, unsubscribe rates | Klaviyo API |
| **Airtable** | Lead pipeline, outreach outcomes, proposal status, deal progression | Airtable API |
| **Shopify** | Conversion rate, AOV, revenue, sample pack sales | Shopify API |
| **GitHub** | Skill updates made, new skills added, skills removed | GitHub API |
| **Agent logs** | Output counts, error rates, compute costs | Read `outputs.jsonl` |

### 2. Generate Scorecards (Automated)

Agent produces one scorecard per agent:

```markdown
# Agent Scorecard — July 2026

## Lead Researcher
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Briefs/week | ≥ 10 | 12 | ✅ |
| Quality score | ≥ 4 | 4.2 | ✅ |
| Conversion to outreach | ≥ 30% | 35% | ✅ |
| Accuracy | ≥ 90% | 94% | ✅ |
| Time saved/brief | ≥ 2 hrs | 2.5 hrs | ✅ |
| **Overall: KEEP** | | | |

## Proposal Writer
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Proposals/week | ≥ 3 | 2 | ⚠️ |
| Acceptance rate | ≥ 20% | 18% | ⚠️ |
| Edit time/proposal | ≤ 10 min | 15 min | ❌ |
| **Overall: IMPROVE** | | | |

## Outreach Sequencer
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Sequences/week | ≥ 5 | 8 | ✅ |
| Open rate | ≥ 40% | 45% | ✅ |
| Reply rate | ≥ 8% | 6% | ⚠️ |
| Meeting booking | ≥ 2% | 1.5% | ⚠️ |
| **Overall: IMPROVE** | | | |

## Inbox Triage
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Emails/day | ≥ 50 | 65 | ✅ |
| Accuracy | ≥ 95% | 97% | ✅ |
| Reply quality | ≥ 80% | 85% | ✅ |
| Time saved/day | ≥ 30 min | 45 min | ✅ |
| **Overall: KEEP** | | | |

## Follow-up Chaser
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Threads tracked | ≥ 20 | 18 | ⚠️ |
| Re-engagement rate | ≥ 15% | 12% | ⚠️ |
| Thread closure | ≥ 30% | 28% | ⚠️ |
| **Overall: IMPROVE** | | | |

## Content Repurposer
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Pieces/week | ≥ 3 | 4 | ✅ |
| LinkedIn engagement | ≥ 2% | 2.8% | ✅ |
| X engagement | ≥ 1% | 0.9% | ⚠️ |
| Newsletter open | ≥ 30% | 32% | ✅ |
| **Overall: KEEP** | | | |

## Weekly Report
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Action rate | ≥ 60% | 55% | ⚠️ |
| Satisfaction | ≥ 4 | 3.8 | ⚠️ |
| Missed items | ≤ 10% | 8% | ✅ |
| **Overall: IMPROVE** | | | |
```

### 3. Compile Patterns Report (Automated)

Agent reads `win-loss.jsonl` and produces `patterns-report.md` with:
- Winning patterns to add to skills
- Losing patterns to remove or avoid
- New hypotheses to test next month
- Skill update recommendations

### 4. Calculate ROI (Automated)

Agent calculates per-agent ROI:

```markdown
# Agent ROI — July 2026

| Agent | Time Saved (hrs/mo) | Value (@ £100/hr) | Compute Cost | Tool Cost | Review Time | Net ROI | Verdict |
|-------|---------------------|-------------------|--------------|-----------|-------------|---------|---------|
| Lead Researcher | 52 | £5,200 | £20 | £0 | 10 min | £5,167 | **KEEP** |
| Proposal Writer | 12 | £1,200 | £20 | £0 | 15 min | £1,158 | **KEEP** |
| Outreach Sequencer | 24 | £2,400 | £20 | £50 | 20 min | £2,297 | **KEEP** |
| Inbox Triage | 90 | £9,000 | £20 | £0 | 10 min | £8,967 | **KEEP** |
| Follow-up Chaser | 8 | £800 | £20 | £0 | 10 min | £753 | **KEEP** |
| Content Repurposer | 16 | £1,600 | £20 | £30 | 15 min | £1,530 | **KEEP** |
| Weekly Report | 4 | £400 | £20 | £0 | 5 min | £367 | **KEEP** |
```

**Threshold:** Any agent with net ROI < £1,000/year gets flagged for review.

## The Review Meeting (30 Minutes, Structured)

### Part 1: Scorecards (10 Minutes)

Colin reviews each agent's scorecard. For each agent marked "IMPROVE":

- What metric is failing?
- Is it a skill issue, a data issue, or a goal issue?
- Quick decision: adjust skill, adjust target, or deprecate agent?

### Part 2: Patterns & Skill Updates (10 Minutes)

Colin reviews the patterns report. For each recommendation:

- ✅ Approve — Make this change
- ❌ Reject — Don't make this change
- 🤔 Test — Run an A/B test before deciding

**Colin doesn't need to read the full report.** The agent surfaces the top 5 recommendations. Colin decides yes/no on each.

### Part 3: Strategic Questions (10 Minutes)

Agent asks Colin 5 pre-loaded questions:

```markdown
## Monthly Review Questions — July 2026

1. **What worked best this month?**
   [Colin answers in 1-2 sentences]
   → Agent notes for next month's focus

2. **What didn't work?**
   [Colin answers in 1-2 sentences]
   → Agent flags for skill review or agent deprecation

3. **What should we test next month?**
   [Colin suggests 1-2 ideas]
   → Agent adds to test queue

4. **Any agents that feel like overhead?**
   [Colin names any that feel like more work than help]
   → Agent calculates ROI. If < £1,000/year, proposes deprecation plan.

5. **Any new agents we need?**
   [Colin names any gap in the swarm]
   → Agent drafts skill requirements and ROI estimate.
```

Colin answers each question in 1-2 sentences. The agent captures the answers and uses them to update priorities.

## After the Review (Automated, 0 Minutes of Colin's Time)

### 1. Update Skills (Based on Colin's Approvals)

Agent makes all approved skill changes and commits to git.

### 2. Update Test Queue

Agent adds new tests based on:
- Colin's "what to test next" answer
- Patterns that were inconclusive
- New hypotheses from the patterns report

### 3. Update Agent Roster

| Decision | Action | Timeline |
|----------|--------|----------|
| **Keep agent** | No action. Continue monitoring. | Ongoing |
| **Improve agent** | Agent drafts skill update plan. Colin reviews next week. | 1 week |
| **Merge agent** | Combine with related agent. Update skills. | 2 weeks |
| **Deprecate agent** | Phase out over 2 weeks. Document learnings. | 2 weeks |
| **Add new agent** | Draft skills, estimate ROI, get Colin's approval. | 2-4 weeks |

### 4. Update Monthly Tracker

Agent writes a summary to `agent-swarm/learning/reviews/YYYY-MM-REVIEW.md`:

```markdown
# Monthly Review — July 2026

## Date: 2026-07-31
## Attendees: Colin + Agent Swarm

## Agent Scorecards
- KEEP: Lead Researcher, Inbox Triage, Content Repurposer
- IMPROVE: Proposal Writer, Outreach Sequencer, Follow-up Chaser, Weekly Report
- DEPRECATE: None
- ADD: None

## Skill Updates Approved
1. ✅ email-sequence: Add "Quick question about [Company]" subject line
2. ✅ linkedin-post-writing: Make story-based hook default
3. ❌ copywriting: Remove formal tone examples (Colin: keep as option)
4. 🤔 proposal-writing: Test bullet-point vs. narrative format (queue for August)

## Colin's Answers
1. What worked: Inbox Triage — saved 45 min/day. Quality is excellent.
2. What didn't: Proposal Writer — edit time too high, acceptance rate low.
3. Test next: Does adding a P.S. to outreach emails improve reply rate?
4. Overhead: Weekly Report — action rate dropping, needs refocus.
5. New agents: None needed right now.

## Actions for August
- [ ] Improve Proposal Writer skill (reduce edit time, increase acceptance)
- [ ] Test P.S. in outreach sequences (EMAIL-PS-001)
- [ ] Refocus Weekly Report on Colin's top 3 priorities only
- [ ] Continue monitoring Outreach Sequencer reply rate

## Test Queue (August)
1. EMAIL-PS-001: P.S. line vs. no P.S. in outreach
2. PROP-FORMAT-001: Bullet-point vs. narrative proposals
3. LI-CTA-001: Question CTA vs. statement CTA
```

## Agent Roster Decisions

### When to Add an Agent

1. Colin identifies a gap (repetitive task, missed opportunity, manual work)
2. Agent drafts skill requirements and ROI estimate
3. Colin approves if estimated ROI > £1,000/year
4. Agent builds skill, tests with 5-10 outputs
5. Colin reviews outputs. If quality ≥ 7/10, agent goes live.

### When to Remove an Agent

1. Scorecard shows consistent underperformance (3+ months)
2. Net ROI < £1,000/year
3. Colin's feedback: "This feels like more work than help"
4. Agent is duplicating another agent's work (merge instead of remove)

### When to Merge Agents

1. Two agents have overlapping responsibilities
2. Combined agent can do both jobs with one skill set
3. Merged agent's ROI > sum of individual ROIs (economies of scale)

**Example:** Follow-up Chaser + Outreach Sequencer could merge into a single "Communication Manager" if both are low-volume.

## Data Pull Checklist (Automated Before Each Review)

- [ ] Notion: Tasks completed, action items, project status
- [ ] Buffer: Social engagement, best posts, follower growth
- [ ] Klaviyo: Email metrics, sequence performance, unsubscribes
- [ ] Airtable: Lead pipeline, outreach outcomes, deal status
- [ ] Shopify: Conversion, revenue, AOV, sample pack sales
- [ ] GitHub: Skill commits, new skills, deprecated skills
- [ ] Agent logs: Output counts, error rates, compute costs
- [ ] Win/loss log: Patterns extracted, recommendations drafted

## Monthly Review Template (For Agent to Generate)

```markdown
# Monthly Review — [Month YYYY]

## Date: [Date]
## Agent Roster Status: [Stable / Growing / Shrinking]

## Scorecards
[One table per agent — target vs. actual vs. verdict]

## ROI Summary
[Total time saved, total cost, net ROI for the swarm]

## Skill Updates
[Approved changes with links to commits]

## Test Results
[Completed tests with winners and rollout status]

## Test Queue (Next Month)
[Queued tests with priority]

## Colin's Feedback
[Answers to the 5 questions]

## Decisions
[Keep / Improve / Merge / Deprecate / Add for each agent]

## Action Items
[Specific tasks for next month with owners and deadlines]
```

## Time Budget

| Task | Colin's Time | Agent's Time |
|------|--------------|--------------|
| Data pull | 0 min | 30 min (automated) |
| Scorecard generation | 0 min | 15 min (automated) |
| Patterns report | 0 min | 20 min (automated) |
| Review meeting | 30 min | 0 min |
| Skill updates (after approval) | 0 min | 30 min (automated) |
| Test queue update | 0 min | 10 min (automated) |
| Review summary | 0 min | 15 min (automated) |
| **Total** | **30 min** | **2 hours** |

## Key Principles

1. **Never skip the monthly review.** It's the heartbeat of the learning system. If it gets skipped, the system stops learning.
2. **Data pulls before, not during.** Colin's 30 minutes is for decisions, not data gathering.
3. **Only discuss agents that need attention.** If an agent is performing well (KEEP), spend 30 seconds on it, not 5 minutes.
4. **Colin's answers drive next month's focus.** The 5 questions aren't bureaucracy — they're the only human input that shapes the swarm's evolution.
5. **If an agent isn't worth £1,000/year, it's not worth having.** This is a business, not a science project. Cut mercilessly.
6. **Document every decision.** The review summary is the audit trail. If Colin asks "why did we change that skill?", the answer is in the review file.

## File Structure

```
agent-swarm/learning/
├── reviews/
│   ├── 2026-07-REVIEW.md
│   ├── 2026-08-REVIEW.md
│   └── ...
├── logs/
│   ├── 2026-07/
│   │   ├── outputs.jsonl
│   │   ├── win-loss.jsonl
│   │   └── patterns-report.md
│   └── 2026-08/
│       └── ...
├── experiments/
│   ├── 2026-07/
│   └── 2026-08/
└── FEEDBACK_LOOP.md
    METRICS_FRAMEWORK.md
    A_B_TESTING.md
    SKILL_IMPROVEMENT.md
    MONTHLY_REVIEW.md  ← This file
```

## Quick Start

To run the first monthly review:

1. ✅ Agent pulls all data (automated)
2. ✅ Agent generates scorecards (automated)
3. ✅ Agent writes patterns report (automated)
4. ✅ Colin reviews for 30 minutes on last Friday of month
5. ✅ Agent makes approved changes (automated)
6. ✅ Agent writes review summary (automated)
7. ✅ Next month's test queue is ready

**First review: 2026-07-31**
