# Inbox Triage Workflow

> **Purpose:** Turn email chaos into a clean, prioritized, actionable queue. Read every unread email, sort it, and draft replies for Colin to approve. Never let an important email slip through.
> **Assigned Agent:** `Agent InboxTriage` (or any agent with the `email-automation` skill + `ai-voice-humanizer` skill)
> **Category:** Operations / Daily Rhythm

---

## Trigger Conditions

| Trigger | How It's Detected | Frequency |
|---------|------------------|-----------|
| **Scheduled Run** | Cron job: every 2 hours during business hours (09:00-18:00 GMT) | Every 2 hours |
| **Manual Run** | Colin says "check my inbox" or runs the triage command | On demand |
| **Volume Threshold** | > 10 unread emails detected | Auto-trigger |
| **Urgent Signal** | Email subject or sender matches urgent keywords (see below) | Real-time (if possible) |
| **End-of-Day** | 18:00 GMT — final sweep to ensure nothing urgent was missed | Daily |

**Urgent Keywords (Real-Time Triggers):**
- Subject contains: `urgent`, `asap`, `deadline`, `invoice`, `payment`, `contract`, `legal`, `complaint`, `refund`, `chargeback`, `meeting`, `call`, `zoom`, `calendly`
- Sender contains: `legal`, `solicitor`, `lawyer`, `hmrc`, `revenue`, `tax`, `accountant`, `bank`, `stripe`, `shopify`, `klaviyo`, `supplier`, `manufacturer`, `3pl`, `lab`, `investor`
- From Colin's known contacts with `priority: high` flag in address book

---

## Input Format

### Required Fields
| Field | Description | Source |
|-------|-------------|--------|
| `inbox_source` | Which email account(s) to check | `colin@vitae10.com` (primary), `colin1miley@gmail.com` (personal), etc. |
| `triage_mode` | `full` (all unread), `urgent-only` (flagged urgent), `summary` (just a digest) | Default: `full` |

### Optional Fields
| Field | Description | Default |
|-------|-------------|---------|
| `max_emails` | Max unread emails to process in one run | 50 (to avoid rate limits and token burn) |
| `sender_whitelist` | Senders to always mark as important | Known suppliers, investors, key partners |
| `sender_blacklist` | Senders to auto-archive (newsletters, marketing, etc.) | Known newsletter domains |
| `colin_context` | What Colin is currently focused on (e.g., "launching Vitae10", "closing Q3 deals") | From `MEMORY.md` or Colin's input |
| `reply_style` | `concise`, `detailed`, `defer` | `concise` |

---

## Step-by-Step Process

### Phase 1: Ingestion & Scanning
- [ ] **1.1 Connect to Email**
  - Authenticate to configured email accounts (Microsoft 365 for `colin@vitae10.com`, Gmail API for personal)
  - Pull unread emails, limited to `max_emails` per run
  - Sort by: received time (newest first), then by sender priority

- [ ] **1.2 Metadata Extraction**
  - For each email, extract: sender, subject, timestamp, thread length, attachments, recipient(s)
  - Detect if it's a new thread or part of an existing thread (check thread ID)
  - Flag if it's a reply to something Colin previously sent

- [ ] **1.3 Content Classification**
  - Read the full email body (plain text preferred)
  - Classify into one of these categories:
    - `inbound-inquiry` — Potential customer, partner, or media inquiry
    - `supplier-comm` — Manufacturer, packager, 3PL, lab communication
    - `platform-alert` — Shopify, Stripe, Klaviyo, HMRC, etc.
    - `newsletter-marketing` — Promotional, newsletters, cold outreach to Colin
    - `personal` — Friends, family, non-business
    - `internal` — From Colin's own agents or systems
    - `spam-phishing` — Obvious spam or suspicious links

### Phase 2: Priority Sorting
- [ ] **2.1 Urgency Assessment**
  - For each email, assign a priority:
    - **`URGENT`**: Requires action within 4 hours. Examples: supplier says "shipment delayed", Stripe chargeback, legal notice, meeting request for today, Vitae10 customer complaint.
    - **`IMPORTANT`**: Requires action within 24 hours. Examples: warm lead reply, partnership inquiry, invoice due, content deadline, agent council review request.
    - **`FYI`**: No action needed, just awareness. Examples: newsletter, platform update, completed task notification, social mention.
    - **`ARCHIVE`**: No value, auto-archive. Examples: cold sales email to Colin, marketing blast, phishing attempt.

- [ ] **2.2 Contextual Boosting**
  - Cross-reference with `colin_context`:
    - If Colin is "launching Vitae10", boost all supplier and logistics emails to `URGENT` or `IMPORTANT`
    - If Colin is "closing Q3 deals", boost all inbound inquiries and meeting requests to `URGENT` or `IMPORTANT`
  - Cross-reference with `MEMORY.md`:
    - If a supplier was recently flagged as "at risk", any email from them gets `URGENT`
    - If a lead was recently marked as `hot`, any reply from them gets `URGENT`

- [ ] **2.3 Sender Intelligence**
  - Known suppliers: boost by 1 priority level (e.g., `FYI` → `IMPORTANT`)
  - Known investors/partners: boost by 1 priority level
  - Known high-value leads: boost to `URGENT` if they reply
  - Newsletter domains: auto-`ARCHIVE` unless flagged by Colin

### Phase 3: Reply Drafting (For URGENT & IMPORTANT Only)
- [ ] **3.1 Context Gathering**
  - For `URGENT` and `IMPORTANT` emails, pull the full thread context (previous emails in the thread)
  - Check if there's a related Airtable record (lead, supplier, partner)
  - Check `MEMORY.md` for any recent notes about this sender or topic

- [ ] **3.2 Draft Reply**
  - For each actionable email, draft a reply in Colin's voice:
    - **Acknowledge:** Show you read and understood the email (specific reference to their point)
    - **Answer:** Address the question or concern directly
    - **Action:** State what happens next (and who does it, if not Colin)
    - **Tone:** Match the sender's tone (formal for legal, casual for partners, warm for customers)
  - If the email requires a decision Colin hasn't made yet, draft a "holding reply" that buys time without being evasive
  - If the email requires action from someone else (e.g., a supplier issue that needs the 3PL), draft a reply that delegates clearly

- [ ] **3.3 AI Voice Humanizer Pass**
  - Run each draft reply through `ai-voice-humanizer`
  - Ensure it sounds like Colin, not a template
  - Check for: over-apologizing, passive voice, excessive formality, corporate filler

- [ ] **3.4 Draft Metadata**
  - For each draft, add:
    - `suggested_action`: `send-now`, `send-later`, `colin-review`, `delegate-to-[agent]`, `no-reply-needed`
    - `proposed_send_time`: If not urgent, suggest a time (e.g., "Send at 09:00 tomorrow for visibility")
    - `confidence`: `high` (straightforward reply), `medium` (needs Colin's judgment), `low` (Colin must decide)

### Phase 4: Output Assembly & Delivery
- [ ] **4.1 Build Triage Report**
  - Compile the full triage report (see Output Format below)
  - Group by priority: URGENT first, then IMPORTANT, then FYI, then ARCHIVE

- [ ] **4.2 Quality Gate**
  - Self-review: Does every URGENT email have a drafted reply? Are there any emails that feel miscategorized?
  - Run report through `ai-voice-humanizer` for the summary text (not the draft replies — those were already humanized)

- [ ] **4.3 Colin Delivery**
  - Post triage report to `#inbox-triage` channel
  - For URGENT emails: @mention Colin immediately with a one-liner summary
  - For IMPORTANT emails: Include in the report with draft replies; Colin can approve or edit
  - For FYI emails: Listed in report; Colin can scan async
  - For ARCHIVE emails: Listed in report; Colin can review if anything was misclassified

- [ ] **4.4 Action Execution**
  - If Colin approves a draft reply (via reaction, reply, or button click), the agent sends it
  - If Colin edits a draft, the agent sends the edited version
  - If Colin says "no reply needed", the agent marks the email as read (no reply sent)
  - If Colin delegates, the agent routes the email to the appropriate agent with full context

---

## Output Format

```markdown
# Inbox Triage Report — [Date] [Time] GMT
**Account:** colin@vitae10.com  
**Emails Processed:** [N] unread  
**Urgent:** [N] | **Important:** [N] | **FYI:** [N] | **Archive:** [N]  
**Triage Time:** [X minutes]

---

## 🚨 URGENT — Action Required Within 4 Hours

### Email 1: [Subject] — [Sender] — [Time]
**Category:** [inbound-inquiry / supplier-comm / platform-alert / etc.]  
**Thread:** [New / Reply to Colin's email]  
**Summary:** [2-3 sentence summary of what they want]  
**Why Urgent:** [Specific reason]

**Draft Reply:**
> [Draft reply in Colin's voice]

**Suggested Action:** [send-now / colin-review / delegate-to-X]  
**Confidence:** [high/medium/low]  
**Proposed Send Time:** [If applicable]

**Colin Action:** [ ] Approve & Send  [ ] Edit  [ ] No Reply Needed  [ ] Delegate

---

### Email 2: ...

## 🔶 IMPORTANT — Action Required Within 24 Hours

### Email 1: [Subject] — [Sender] — [Time]
**Category:** [category]  
**Summary:** [2-3 sentence summary]  
**Why Important:** [Specific reason]

**Draft Reply:**
> [Draft reply]

**Suggested Action:** [send-now / send-later / colin-review / delegate]  
**Confidence:** [high/medium/low]

**Colin Action:** [ ] Approve & Send  [ ] Edit  [ ] No Reply Needed  [ ] Delegate

---

## ℹ️ FYI — No Action Needed (Awareness)

- [Subject] — [Sender] — [Summary in one line]
- [Subject] — [Sender] — [Summary in one line]

## 🗑️ ARCHIVE — Auto-Archived (Review if Needed)

- [Subject] — [Sender] — [Why archived]
- [Subject] — [Sender] — [Why archived]

---

## Quick Stats
- **Avg. Triage Time per Email:** [X seconds]
- **Reply Drafts Generated:** [N]
- **High Confidence Drafts:** [N]
- **Low Confidence (Colin Must Decide):** [N]
- **Known Suppliers Contacted:** [List]
- **New Leads Detected:** [List]

## Agent Notes
- [Any observations, patterns, or anomalies the agent noticed]
- [e.g., "Supplier X has emailed 3 times in 2 days about the same issue — may be escalating"]
- [e.g., "Three inbound inquiries this week all mention the same competitor — worth noting"]
```

---

## Quality Gate

| Gate | Required? | Criteria |
|------|-----------|----------|
| **Classification Accuracy** | Always | Every email must have a category and priority. No unclassified emails. |
| **Contextual Boosting** | Always | Context from `colin_context` and `MEMORY.md` must be applied to adjust priorities. |
| **AI Voice Humanizer** | Always (for draft replies) | Replies sound like Colin. No template language. No passive-aggressive tone. |
| **Self-Review** | Always | Agent double-checks: Are any URGENT emails missing a draft? Are any priorities clearly wrong? |
| **Colin Approval** | Always (for sending) | No reply is sent without Colin's explicit approval. Drafts are presented; Colin decides. |

---

## Metrics to Track

### Per-Run Metrics (Tracked in Daily Log)
| Metric | How Measured | Target |
|--------|-------------|--------|
| **Triage Speed** | Total unread emails processed / time taken | < 30 seconds per email |
| **Classification Accuracy** | Colin's feedback: "Was this priority correct?" | > 90% |
| **Draft Quality** | Colin's edits: How many words changed per draft? | < 20% edit rate |
| **Urgent Detection** | False negatives: URGENT emails missed | 0% (zero tolerance) |
| **False Positive Rate** | FYI emails marked as URGENT | < 5% |
| **Reply Draft Coverage** | % of URGENT/IMPORTANT emails with a draft reply | 100% |
| **Colin Approval Rate** | % of drafts Colin approves without edits | > 60% |

### System-Level Metrics (Tracked Weekly)
| Metric | How Measured | Target |
|--------|-------------|---------|
| **Inbox Zero Frequency** | % of days where inbox is triaged to zero by EOD | > 80% |
| **Response Time** | Time from email arrival → Colin's reply sent | < 4 hours for URGENT, < 24 hours for IMPORTANT |
| **Lead Detection Rate** | Inbound inquiries correctly identified as leads | > 95% |
| **Supplier Escalation Detection** | Supplier issues flagged before they become critical | 100% |
| **Newsletter Noise** | % of newsletters correctly auto-archived | > 95% |
| **Agent Delegation Rate** | % of emails Colin delegates to other agents vs. handling himself | Trending up (goal: free Colin for decisions) |

### Self-Learning Feedback Loop
- **Per-run:** Colin provides feedback on classification accuracy (reaction or quick note). Feed into classification model.
- **Weekly:** Review misclassified emails. What patterns caused the error? (New sender type? Ambiguous subject? Missing context?) Update classification rules.
- **Weekly:** Review draft replies Colin heavily edited. What was the pattern? (Wrong tone? Missing context? Wrong assumption?) Update drafting heuristics.
- **Monthly:** Re-evaluate urgency keywords. Are there new sender types or subject patterns that should trigger URGENT? Update keyword list.
- **Monthly:** Review auto-archive performance. Did anything important get archived? Did anything spam get through? Update whitelist/blacklist.
- **Quarterly:** Re-evaluate the triage cadence. Is every 2 hours right? Should it be every hour during launch periods? Should it be every 4 hours during quiet periods?
- **Continuous:** Build a "sender intelligence" database. For each sender, track: priority history, Colin's typical response time, common topics, relationship status. Use this to improve future triage.

---

## Interdependencies

| Upstream | Downstream | Data Handoff |
|----------|-----------|--------------|
| **External Inbox** | This workflow | Raw unread emails → sorted, prioritized, with draft replies |
| This workflow | **Lead Researcher** | Inbound inquiries flagged as leads → auto-trigger research (if not already researched) |
| This workflow | **Outreach Sequencer** | Replies to outreach sequences are triaged; "Interested" replies trigger meeting booking or next sequence step |
| This workflow | **Follow-up Chaser** | Replies that require follow-up (e.g., "I'll get back to you next week") are added to the follow-up pipeline |
| This workflow | **Weekly Report** | Triage metrics (emails processed, leads detected, urgent issues resolved) are aggregated into the Monday report |
| This workflow | **Proposal Writer** | Inbound RFPs or formal inquiries are flagged and routed to Proposal Writer after brief generation |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-01 | Initial workflow design |

