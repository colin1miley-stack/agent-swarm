# A/B Testing — How to Run Experiments

> Test one variable at a time. Get signal before declaring a winner. Document everything. Roll out winners, kill losers.

## The Rule: One Variable Per Test

Never test subject line AND CTA AND timing in the same experiment. You won't know which change drove the result.

| What You Can Test | Examples | What You Can't Do |
|-------------------|----------|-------------------|
| Subject line | "Quick question about [Company]" vs. "Partnership opportunity" | Test subject line + CTA + tone all at once |
| CTA button text | "Book a call" vs. "See how it works" | Test CTA + email body + sender name |
| Email tone | Casual vs. formal | Test tone + length + personalization |
| Send timing | Tuesday 9am vs. Thursday 2pm | Test timing + day + frequency |
| LinkedIn hook | Question vs. Statistic vs. Story | Test hook + format + length |
| CTA placement | End of post vs. middle | Test placement + wording + emoji |
| Personalization level | First name only vs. company + role + recent news | Test level + depth + source |
| Follow-up timing | 3 days vs. 7 days | Test timing + number of follow-ups + tone |

## The Process

### Step 1: Hypothesis (2 minutes)

Before running a test, write one sentence:

> **Hypothesis:** Changing the subject line from generic to personalized will increase open rate by 10%.

If you can't write a clear hypothesis, don't run the test.

### Step 2: Define Variants (2 minutes)

```
Variant A (Control): Current approach
Variant B (Test): One change from hypothesis
```

**Maximum 2 variants for most tests.** Only run A/B/C if you have high volume (100+ samples/week).

### Step 3: Random Assignment (Automatic)

The agent assigns each output to a variant:

```python
# Simple: alternate A, B, A, B...
variant = "A" if output_count % 2 == 0 else "B"

# Or: weighted for unequal splits
variant = random.choices(["A", "B"], weights=[0.5, 0.5])[0]
```

**The variant is logged in `outputs.jsonl` so you can trace every result back to its test group.**

### Step 4: Minimum Sample Size

Don't declare a winner until you have enough data. Use these minimums:

| Metric | Minimum Sample (per variant) | Why |
|--------|------------------------------|-----|
| Open rate | 50 sends | 50% open rate has ±14% margin of error |
| Reply rate | 100 sends | 5% reply rate has ±4% margin of error |
| Meeting booking | 200 sends | 2% booking rate needs 200 for signal |
| Engagement rate | 30 posts | Social has high variance |
| Conversion rate | 500 visitors | 2% conversion needs 500 for 95% confidence |

**Rule of thumb:** If you can't get the minimum sample in 2 weeks, the test is too low-volume to be meaningful. Either run it longer or don't run it.

### Step 5: Statistical Significance (Simplified)

You don't need a stats degree. Use this simple rule:

```
Win rate difference ≥ 20% AND minimum sample met = Probably real
Win rate difference ≥ 50% AND minimum sample met = Definitely real
Win rate difference < 20% OR sample too small = Inconclusive, keep testing or move on
```

**Example:**
- Variant A: 40% open rate (50 sends) = 20 opens
- Variant B: 52% open rate (50 sends) = 26 opens
- Difference: 12 percentage points (30% relative improvement)
- Verdict: Promising but not definitive. Run to 100 sends each.

- Variant A: 38% open rate (100 sends)
- Variant B: 55% open rate (100 sends)
- Difference: 17 percentage points (45% relative improvement)
- Verdict: Strong signal. Winner: B.

### Step 6: Document Results

Every test gets an entry in `agent-swarm/learning/experiments/YYYY-MM/TEST-ID.md`:

```markdown
# Test: EMAIL-SUBJ-2026-07-01

## Hypothesis
Personalizing subject line with company name will increase open rate by 10%.

## Variants
- **A (Control):** "Partnership opportunity with Vitae10"
- **B (Test):** "Quick question about [Company Name]"

## Results
| Metric | A (Control) | B (Test) | Winner |
|--------|-------------|----------|--------|
| Sends | 100 | 100 | — |
| Opens | 38 (38%) | 55 (55%) | B |
| Replies | 3 (3%) | 8 (8%) | B |
| Meetings | 0 (0%) | 2 (2%) | B |

## Winner: B

## Rollout
- Rolled out to all outreach sequences on 2026-07-15
- Updated email-sequence skill with new subject line template
- Added to skill version 1.3 (see git commit a1b2c3d)

## Notes
- B also had higher reply rate, suggesting subject line quality affects engagement beyond opens
- Next test: test personalized subject line with vs. without "Quick question" framing
```

## Running Tests: Practical Examples

### Email Subject Line Test

```
Test: EMAIL-SUBJ-001
Hypothesis: Curiosity-gap subject lines outperform direct benefit statements

A: "How we reduced churn by 30%" (direct benefit)
B: "The churn metric most SaaS founders ignore" (curiosity gap)

Min sample: 100 per variant
Run time: 2 weeks
Winner threshold: 20% open rate difference
```

### LinkedIn Hook Test

```
Test: LI-HOOK-001
Hypothesis: Story-based hooks outperform statistic-based hooks

A: "73% of sales teams waste 4 hours/week on data entry." (statistic)
B: "Last Tuesday, my CRM crashed mid-pitch. Here's what I learned." (story)

Min sample: 30 posts per variant
Run time: 2 weeks
Winner threshold: 20% engagement rate difference
```

### CTA Test

```
Test: CTA-001
Hypothesis: Low-commitment CTAs outperform high-commitment CTAs

A: "Book a 30-minute demo" (high commitment)
B: "See how it works in 2 minutes" (low commitment)

Min sample: 200 per variant
Run time: 4 weeks
Winner threshold: 20% click rate difference
```

### Follow-up Timing Test

```
Test: FUP-TIME-001
Hypothesis: 7-day follow-up outperforms 3-day follow-up (less pushy)

A: Follow up at 3 days
B: Follow up at 7 days

Min sample: 100 per variant
Run time: 3 weeks
Winner threshold: 20% re-engagement rate difference
```

## Test Queue

Maintain a backlog of test ideas:

| Priority | Test ID | Hypothesis | Agent | Status |
|----------|---------|------------|-------|--------|
| 1 | EMAIL-SUBJ-001 | Curiosity gap > direct benefit | Outreach | Running |
| 2 | LI-HOOK-001 | Story > statistic | Content | Queued |
| 3 | CTA-001 | Low commitment > high commitment | Outreach | Queued |
| 4 | FUP-TIME-001 | 7 days > 3 days | Follow-up | Queued |
| 5 | PROP-TONE-001 | Casual > formal | Proposal | Backlog |

**Priority rules:**
1. Tests with highest potential impact (reply rate, meeting booking, conversion)
2. Tests that are easiest to run (one-line change, no new infrastructure)
3. Tests that build on previous winners (compound improvements)

## What to Do With Results

| Result | Action | Timeline |
|--------|--------|----------|
| **Clear winner** (B beats A by ≥20%, min sample met) | Roll out B to all future outputs. Update skill. | Within 1 week |
| **Weak winner** (B beats A by 10-20%, min sample met) | Consider rolling out, but queue a confirmatory test. | Next month |
| **Inconclusive** (difference <10% or sample too small) | Don't change anything. Either run longer or abandon. | Re-evaluate in 2 weeks |
| **Clear loser** (B worse than A by ≥20%) | Kill B. Document why it failed. Don't repeat. | Immediately |
| **Surprising result** (B wins on one metric but loses on another) | Dig deeper. Was there a segment effect? Document. | Within 1 week |

## Anti-Patterns to Avoid

❌ **Testing too many things at once** — You won't know what worked
❌ **Declaring winners too early** — Wait for minimum sample size
❌ **Ignoring losers** — Document why they failed to avoid repeating
❌ **Running tests forever** — 2-4 weeks max. Then decide and move on.
❌ **Testing without a hypothesis** — "Let's see what happens" is not a test, it's a guess
❌ **Not rolling out winners** — What's the point of testing if you don't use the winner?
❌ **Testing trivial changes** — "Should the CTA be blue or green?" — test things that matter

## Test Documentation Template

```markdown
# Test: [ID]

## Hypothesis
[One sentence, clear and falsifiable]

## Variants
- A: [Control — current approach]
- B: [Test — one change]

## Success Criteria
[What metric must improve by how much?]

## Minimum Sample
[X per variant]

## Run Dates
[Start] — [End]

## Results
[Table with all metrics]

## Winner
[A / B / Inconclusive]

## Rollout / Kill Decision
[What we did and when]

## Next Test
[What this test suggests we should try next]
```

## File Structure

```
agent-swarm/learning/experiments/
├── 2026-07/
│   ├── EMAIL-SUBJ-001.md
│   ├── LI-HOOK-001.md
│   └── CTA-001.md
├── 2026-08/
│   └── ...
├── test-queue.md           ← Backlog of test ideas
└── README.md               ← How to run a test (this doc, condensed)
```

## Quick Reference: Test Checklist

Before starting a test:
- [ ] One clear hypothesis written down
- [ ] Only one variable different between variants
- [ ] Minimum sample size calculated
- [ ] Success criteria defined (what metric, what improvement)
- [ ] Test documented in `experiments/YYYY-MM/TEST-ID.md`

After a test:
- [ ] Results filled in
- [ ] Winner declared or test marked inconclusive
- [ ] Winner rolled out or loser killed
- [ ] Skill updated if winner found
- [ ] Next test queued based on learnings

**Total time per test (Colin's involvement): ~10 minutes to review and approve.**
