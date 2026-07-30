# Lead Researcher Workflow

> **Purpose:** Transform a raw lead (company name, URL, or person) into a structured intelligence brief that fuels every downstream workflow — proposals, outreach, and follow-ups.
> **Assigned Agent:** `Agent LeadResearcher` (or any agent with the `lead-research` skill)
> **Category:** Ingestion / Pre-Engagement

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Manual Input** | Colin drops a name/URL into the `#lead-research` channel or Airtable base | On demand |
| **Inbox Signal** | Inbox Triage agent flags an inbound inquiry as "high-value prospect" | Real-time |
| **Weekly Scan** | Automated scan of Colin's LinkedIn network for new connections with relevant titles | Every Monday |
| **Competitor Trigger** | Competitor Monitoring agent identifies a company hiring for roles Colin's services address | As detected |
| **Event Pipeline** | Post-webinar or conference: attendee list uploaded for batch research | Post-event |

**Auto-run threshold:** If a company URL is provided and the domain has not been researched in the past 30 days, auto-execute. If a personal name is provided without company context, pause for Colin to confirm the target before proceeding.

---

## Input Format

### Required Fields
| Field | Description | Example |
|-------|-------------|---------|
| `lead_identifier` | Company URL, LinkedIn company page, or person's name + company | `acme-corp.com` or `Jane Doe @ Acme Corp` |
| `source` | Where this lead came from | `linkedin-outbound`, `inbound-inquiry`, `event`, `manual` |
| `colin_context` | What service Colin would pitch them | `ai-automation-consulting`, `[Your Brand]-b2b`, `content-system` |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `priority` | `hot`, `warm`, `cold` | `warm` |
| `known_contact` | Name of the decision-maker if already known | `null` |
| `relevant_angle` | A specific pain point or angle Colin wants to emphasize | `null` (agent will derive) |
| `budget_indicator` | Any signal about budget (funding round, hiring spree, etc.) | `null` |

**Input source:** Airtable (primary), `#lead-research` Slack/Discord channel, or direct message to the agent.

---

## Step-by-Step Process

### Phase 1: Data Harvesting (Automated)
- [ ] **1.1 Company Discovery**
  - Scrape company website via `defuddle` (description, team page, careers, blog)
  - Pull LinkedIn company page data (headcount, industry, recent posts, job openings)
  - Check Crunchbase / OpenCorporates for funding, founding date, key people
  - Check BuiltWith / SimilarTech for tech stack signals

- [ ] **1.2 Signal Detection**
  - Scan for hiring signals (are they hiring sales ops, marketing automation, RevOps roles?)
  - Scan for growth signals (recent funding, new office, product launches)
  - Scan for pain signals (negative reviews, high churn roles, "we're looking for" posts)
  - Check their content: blog topics, webinar titles, case studies (what do they care about?)

- [ ] **1.3 Decision-Maker Mapping**
  - Identify the most likely budget holder for Colin's service type
  - Map title → seniority → likelihood of being a decision-maker
  - Find their LinkedIn, recent posts, and any mutual connections with Colin
  - Check for recent job changes (new hires are more open to change)

### Phase 2: Analysis & Briefing (Agent-Led)
- [ ] **2.1 Company Profile Synthesis**
  - Summarize: What do they do, who do they serve, how do they make money?
  - Size estimate: Revenue range (if inferable), headcount, stage (startup/SMB/enterprise)
  - Market position: Incumbent vs challenger, local vs global, B2B vs B2C

- [ ] **2.2 Pain Point Derivation**
  - From hiring signals: What problems are they trying to solve by hiring?
  - From tech stack: What gaps exist? (e.g., no CRM = sales chaos, no automation = manual everything)
  - From content: What are they publicly struggling with or learning about?
  - From reviews: What do customers or employees complain about?
  - Cross-reference with Colin's service offerings: Which pain point is most addressable?

- [ ] **2.3 Angle Prioritization**
  - Generate 3-5 potential angles (e.g., "AI automation for their manual sales process", "content system to support their hiring spree")
  - Rank by: relevance to Colin's skills, evidence strength, urgency of the pain, competitive differentiation
  - Select the top angle as the primary hook, with 1-2 backups

- [ ] **2.4 Personalization Data Mining**
  - Find the decision-maker's recent LinkedIn posts, articles, podcast appearances
  - Extract specific topics, opinions, or challenges they've discussed publicly
  - Identify mutual connections or shared communities (if any)
  - Note any recent wins, milestones, or personal brand signals (e.g., "building in public")

### Phase 3: Output Assembly
- [ ] **3.1 Draft Brief**
  - Compile all findings into the structured brief template (see Output Format)
  - Tag with confidence levels for each data point (high / medium / low)
  - Flag any gaps that need manual verification (e.g., "Revenue inferred from headcount — could be off by 2x")

- [ ] **3.2 Self-Review**
  - Agent reads its own brief and asks: "Would I feel confident pitching this to Colin?"
  - Check for hallucinations: Are any claims unsupported by the source material?
  - Verify all URLs are reachable and timestamps are current

- [ ] **3.3 Quality Gate Submission**
  - Submit to `Humanizer Pass` (AI Voice Humanizer skill) to ensure tone is sharp, not robotic
  - Submit to `Council Review` (kimi-council skill) if priority is `hot` or confidence is low on key data points
  - Council can: approve, request re-research on specific points, or reject with reasons

- [ ] **3.4 Delivery**
  - Post brief to `#lead-research-output` channel with link to Airtable record
  - Notify Colin with priority tag: `@colin Lead brief ready: [Company] — [Priority]`
  - If `hot`: include a "next step" suggestion (e.g., "Ready for Proposal Writer — click to generate")

---

## Output Format

```markdown
# Lead Brief: [Company Name]
**Generated:** [ISO timestamp]  
**Lead ID:** [Airtable record ID]  
**Priority:** [hot/warm/cold]  
**Confidence:** [high/medium/low] — overall data quality

## Company Snapshot
- **Name:** [Official name]
- **Website:** [URL]
- **Industry:** [Primary + secondary]
- **Headcount:** [LinkedIn estimate]
- **Stage:** [Startup / Scale-up / SMB / Enterprise / Unknown]
- **Location:** [HQ + any relevant offices]
- **Revenue Estimate:** [If inferable, with method noted]
- **Tech Stack:** [Key tools detected — CRM, marketing automation, etc.]

## What They Do (One-Sentence)
[Company] helps [target audience] [achieve outcome] by [core mechanism].

## Pain Points (Ranked by Evidence)
1. **[Pain Point]** — Evidence: [specific signal]. Relevance to Colin: [high/medium/low].
2. **[Pain Point]** — Evidence: [specific signal]. Relevance to Colin: [high/medium/low].
3. ...

## Recommended Angles (Ranked)
1. **[Primary Angle]** — Why it works: [reasoning]. Suggested opening line: [hook].
2. **[Backup Angle 1]** — When to use: [context].
3. **[Backup Angle 2]** — When to use: [context].

## Decision-Maker Profile
- **Name:** [Name]
- **Title:** [Title]
- **LinkedIn:** [URL]
- **Seniority:** [IC / Manager / Director / VP / C-level]
- **Likely Budget Authority:** [Yes / No / Shared with X]
- **Recent Signals:** [Recent posts, job changes, public statements]
- **Personalization Nuggets:** [Specific topics to reference in outreach]

## Competitive Context
- [Are they already working with someone in Colin's space? If so, who?]
- [What makes Colin's approach different?]

## Data Sources
- [List all URLs and tools used, with timestamps]
- [Flag any paywalled or inferred data]

## Gaps & Risks
- [What we don't know that could change the pitch]
- [Any data that needs Colin's manual verification]

## Suggested Next Step
- [Auto-generated recommendation: "Generate proposal", "Send outreach sequence", "Colin to verify angle first", etc.]
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Humanizer Pass** | Always | Brief reads like a sharp research analyst, not a robot. No generic filler. Specific names, numbers, and URLs where possible. |
| **Council Review** | If `priority == hot` OR confidence < medium on key fields | 2+ council members review. Threshold: unanimous approval or 2/3 majority with documented dissent. |
| **Colin Approval** | If `priority == hot` | Colin must read and approve before any downstream workflow triggers. |
| **Auto-Approval** | If `priority == warm` AND confidence >= medium | Brief auto-posts to output channel; Colin can review async. |

---

## Metrics to Track

### Per-Lead Metrics (Tracked in Airtable)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Research Time** | Start → Brief delivery timestamp | < 10 minutes for warm leads, < 5 minutes for hot leads |
| **Data Confidence Score** | Average of self-rated confidence per field | > 70% |
| **Hallucination Rate** | Manual audit of 10% of briefs per week | < 2% unsupported claims |
| **Colin Approval Rate** | % of briefs Colin approves without edits | > 80% (if too low, research methodology needs tuning) |
| **Brief-to-Proposal Conversion** | % of briefs that generate a proposal | > 60% |
| **Brief-to-Meeting Conversion** | % of briefs that result in a meeting booked | > 15% |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Coverage Rate** | % of leads in pipeline with a completed brief | > 95% |
| **Freshness** | % of briefs < 30 days old | > 90% |
| **Angle Accuracy** | Colin's post-meeting feedback: "Was the primary angle correct?" | > 70% |
| **Source Reliability** | Which data sources produce the most actionable insights? | Reviewed monthly |

### Self-Learning Feedback Loop
- **Weekly:** Review which briefs led to meetings. What did they have in common? Update the "Pain Point Derivation" checklist.
- **Monthly:** Audit rejected briefs. Why were they rejected? (Bad data? Wrong angle? Company too small?) → Update research methodology.
- **Quarterly:** Re-evaluate data sources. Are there new tools or signals that should be added? Are any sources consistently producing noise?
- **Continuous:** When Colin closes a deal, trace back to the originating brief. Tag the brief as a "win". Feed winning patterns back into the agent's prioritization model.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **Inbox Triage** | This workflow | Inbound inquiry flagged as prospect → auto-trigger research |
| **Competitor Monitoring** | This workflow | Competitor hiring/growth signals → research their target customers |
| This workflow | **Proposal Writer** | Brief becomes the primary input for proposal customization |
| This workflow | **Outreach Sequencer** | Brief's angles, decision-maker profile, and personalization nuggets feed the sequence |
| This workflow | **Follow-up Chaser** | Decision-maker contact details and engagement history feed the follow-up pipeline |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

