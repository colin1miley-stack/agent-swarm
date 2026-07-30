# Skill Improvement — How Winning Patterns Get Baked Into Skills

> Winning patterns from the feedback loop get extracted and added to skills. Losing patterns get removed. Skills evolve based on evidence, not guesswork.

## The Flow

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   WIN/LOSS LOG  │────▶│ PATTERN EXTRACTION│───▶│  SKILL UPDATE   │
│  (from agent)   │     │  (automated)     │     │  (Colin approves)│
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                                          │
                          ┌─────────────────────────────────┘
                          ▼
                   ┌─────────────────┐
                   │  GIT COMMIT     │
                   │  (versioned)    │
                   └─────────────────┘
```

## What Gets Extracted

### From Wins

| Pattern Type | Example | Where It Goes |
|--------------|---------|---------------|
| **Winning subject line** | "Quick question about [Company]" — 55% open rate | `email-sequence` skill, "Subject Lines" section |
| **Winning hook** | Story-based opener: "Last Tuesday, my CRM crashed..." — 4.2% engagement | `linkedin-post-writing` skill, "Hooks" section |
| **Winning CTA** | "See how it works in 2 minutes" — 12% click rate | `copywriting` skill, "CTAs" section |
| **Winning tone** | Casual, first-person voice gets 30% more replies | `persona.md` file, "Voice" section |
| **Winning timing** | Thursday 2pm sends get 15% more opens than Tuesday 9am | `email-sequence` skill, "Send Timing" section |
| **Winning format** | Bullet-point proposals get accepted 2x more than paragraphs | `proposal-writing` skill, "Format" section |
| **Winning personalization** | Company name in subject line > first name in body | `email-sequence` skill, "Personalization" section |

### From Losses

| Pattern Type | Example | What Happens |
|--------------|---------|--------------|
| **Losing subject line** | "Partnership opportunity" — 28% open, 0% reply | Removed from examples. Added to "Avoid" list. |
| **Losing hook** | Statistic opener: "73% of..." — 1.1% engagement | Removed from examples. Note: "Test story vs. statistic in next iteration." |
| **Losing CTA** | "Book a 30-minute demo" — 2% click | Removed from examples. Replaced with winner. |
| **Losing tone** | Formal corporate voice gets 50% fewer replies | Marked as deprecated in `persona.md`. New default: casual. |
| **Losing timing** | Monday morning sends — lowest open rate all week | Removed from recommended send times. |
| **Losing format** | Long paragraph proposals — 0% acceptance | Replaced with bullet-point format. |
| **Losing length** | 500-word LinkedIn posts — 50% less engagement than 150-word | Added to skill: "Keep LinkedIn posts under 200 words." |

## The Extraction Process (Automated)

### Step 1: Read Win/Loss Logs

Agent reads `agent-swarm/learning/logs/YYYY-MM/win-loss.jsonl` for the month.

### Step 2: Group by Skill and Pattern

```python
# Pseudo-code for pattern extraction
for entry in win_loss_log:
    skill = entry.skill
    pattern = extract_pattern(entry.content)  # e.g., subject line, hook, CTA
    
    if entry.outcome == "win":
        wins[skill][pattern].append(entry.metrics)
    else:
        losses[skill][pattern].append(entry.metrics)
```

### Step 3: Calculate Win Rates

```python
for skill, patterns in all_patterns:
    for pattern, outcomes in patterns:
        win_rate = len(outcomes.wins) / len(outcomes.total)
        if len(outcomes.total) >= 10:  # Minimum evidence
            report[pattern] = {
                "win_rate": win_rate,
                "sample_size": len(outcomes.total),
                "metrics": outcomes.metrics
            }
```

### Step 4: Write Patterns Report

Agent writes `agent-swarm/learning/logs/YYYY-MM/patterns-report.md`:

```markdown
# Patterns Report — July 2026

## email-sequence Skill

### Winning Patterns
1. **Subject line: "Quick question about [Company]"**
   - Win rate: 78% (7 wins / 9 total)
   - Avg open rate: 52% vs. 35% for other subject lines
   - Action: Add to skill examples, make default for new sequences

2. **Personalization: Company name in subject line**
   - Win rate: 65% (13 wins / 20 total)
   - Avg reply rate: 9% vs. 3% without personalization
   - Action: Add to "Personalization" section

### Losing Patterns
1. **Subject line: "Partnership opportunity"**
   - Win rate: 12% (2 wins / 17 total)
   - Avg open rate: 28%, reply rate: 0%
   - Action: Remove from examples. Add to "Avoid" list.

2. **Send time: Monday 9am**
   - Win rate: 22% (4 wins / 18 total)
   - Action: Remove from recommended send times. Default to Thursday 2pm.

## linkedin-post-writing Skill

### Winning Patterns
1. **Hook: Story-based opener**
   - Win rate: 71% (10 wins / 14 total)
   - Avg engagement: 4.2% vs. 1.8% for statistic hooks
   - Action: Make story-based hook the default. Add examples to skill.

### Losing Patterns
1. **Length: 400+ words**
   - Win rate: 25% (3 wins / 12 total)
   - Avg engagement: 1.1% vs. 3.8% for 150-200 word posts
   - Action: Add hard limit: "LinkedIn posts must be 150-250 words."

## Next Month's Hypotheses
1. Test: Does adding "I noticed you recently..." to the first sentence improve reply rate?
2. Test: Does ending with a question vs. a statement improve engagement?
3. Test: Does sending at 11am vs. 2pm make a difference?
```

### Step 5: Colin Reviews (10 Minutes)

Colin reads the report and marks each recommendation:
- ✅ **Approve** — Make this change
- ❌ **Reject** — Don't make this change (keep old approach)
- 🤔 **Test** — Run an A/B test before deciding

## How Skills Get Updated

### Example 1: Email-Sequence Skill

**Winning pattern found:** "Quick question about [Company]" subject line wins 78% of the time.

**Current skill (excerpt):**
```markdown
### Subject Line Examples
- "Partnership opportunity with {{company_name}}"
- "Introducing [Your Brand] — [your product/service]"
```

**Proposed update:**
```markdown
### Subject Line Examples
- "Quick question about {{company_name}}" ← **WINNER: 78% win rate, 52% open rate**
- "Introducing [Your Brand] — [your product/service]"

### Avoid
- "Partnership opportunity" ← **LOSER: 12% win rate, 28% open rate, 0% reply rate**
```

**Colin approves** → Agent makes the edit → Agent commits to git.

### Example 2: LinkedIn Post Writing Skill

**Winning pattern found:** Story-based hooks get 71% win rate.

**Current skill:**
```markdown
### Hook Types
1. Statistic: "73% of sales teams..."
2. Question: "What if you could..."
3. Bold claim: "AI will replace..."
```

**Proposed update:**
```markdown
### Hook Types (Priority Order)
1. **Story:** "Last [day], [thing happened]. Here's what I learned." ← **WINNER: 71% win rate, 4.2% engagement**
2. **Question:** "What if you could..."
3. **Bold claim:** "AI will replace..."
4. **Statistic:** "73% of..." ← **Use sparingly — 29% win rate, 1.8% engagement**
```

### Example 3: Persona.md Voice Update

**Winning pattern found:** Casual, first-person voice gets 30% more replies.

**Current persona.md:**
```markdown
## Voice
- Professional but approachable
- Use "we" when referring to [Your Brand]
- Avoid slang and overly casual language
```

**Proposed update:**
```markdown
## Voice
- **Casual and conversational** ← **WINNER: 30% more replies than formal tone**
- Use "I" and "my" — first person builds connection
- Write like you're talking to a friend at a coffee shop
- Use contractions ("don't", "can't", "here's")
- Avoid corporate speak ("leverage", "synergize", "optimize")
```

## Version Control for Skills

Every skill update is tracked in git with a descriptive commit message:

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9
Author: agent-swarm <agent@swarm.local>
Date:   2026-07-31

    skill(email-sequence): Update subject line defaults based on July data
    
    - Added "Quick question about [Company]" as default subject line
      (78% win rate, 52% open rate from 9 tests)
    - Moved "Partnership opportunity" to "Avoid" list
      (12% win rate, 0% reply rate from 17 tests)
    - Added note: company name personalization beats first-name personalization
    
    Based on patterns report: logs/2026-07/patterns-report.md
    Approved by: Colin (2026-07-31)
```

**Skill version in metadata:**
```markdown
---
version: 1.3
last_updated: 2026-07-31
changes:
  - "Added winning subject line pattern (July 2026 data)"
  - "Deprecated 'Partnership opportunity' subject line"
  - "Updated personalization priority: company name > first name"
---

# Skill: Email Sequence
```

## Update Rules

| Condition | Action | Requires Approval |
|-----------|--------|-------------------|
| Win rate ≥ 70% and sample ≥ 10 | Add to skill as "recommended" or "default" | ✅ Yes |
| Win rate 50-70% and sample ≥ 10 | Add to skill as "option" or "tested" | ✅ Yes |
| Win rate < 30% and sample ≥ 10 | Move to "Avoid" or remove | ✅ Yes |
| Sample < 10 | Note in skill but don't change defaults | ❌ No — just document |
| Inconclusive (30-50% win rate) | No change. Queue for further testing. | ❌ No |

## Skill Update Checklist

- [ ] Pattern extracted from win/loss log with ≥ 10 samples
- [ ] Win rate calculated and meets threshold
- [ ] Patterns report written by agent
- [ ] Colin reviewed and approved/rejected/tested
- [ ] Skill file updated with new pattern
- [ ] "Avoid" list updated with losing patterns
- [ ] Skill metadata version bumped
- [ ] Git commit with descriptive message and link to report
- [ ] Test queued for next hypothesis (if applicable)

## What NOT to Update

Some things shouldn't be changed based on short-term data:

| Don't Change | Why | Exception |
|--------------|-----|-----------|
| Brand voice fundamentals | 6+ months of data needed | If every test loses, reconsider |
| Legal/compliance language | Must be accurate regardless of win rate | Never change without legal review |
| Core product positioning | Strategic, not tactical | If market shifts, reconsider |
| Pricing in proposals | Business decision, not marketing test | A/B test pricing separately |
| Agent's core purpose | If the agent isn't working, cut it, don't redefine it | If 3 months of bad ROI, re-evaluate |

## Quick Reference: Skill Update Types

| Type | Example | Evidence Needed | Colin Time |
|------|---------|-----------------|------------|
| **Add example** | Add winning subject line to skill | 10+ wins, ≥70% win rate | 2 min |
| **Remove example** | Remove losing CTA from skill | 10+ losses, ≤30% win rate | 2 min |
| **Change default** | Make winning approach the default | 20+ tests, ≥60% win rate | 5 min |
| **Add constraint** | "Keep posts under 250 words" | 15+ tests, consistent pattern | 3 min |
| **Update persona** | Change voice from formal to casual | 20+ tests across multiple skills | 10 min |
| **Deprecate skill** | Remove entire skill section | 30+ tests, consistently losing | 15 min |

## File Structure

```
agent-swarm/skills/
├── email-sequence/
│   ├── SKILL.md                    ← Updated with winning patterns
│   ├── examples/
│   │   ├── winning-subjects.md     ← Extracted from win/loss log
│   │   └── losing-subjects.md      ← "Avoid" list
│   └── version-history.md          ← Changelog of all updates
├── linkedin-post-writing/
│   ├── SKILL.md
│   ├── examples/
│   │   ├── winning-hooks.md
│   │   └── losing-hooks.md
│   └── version-history.md
├── copywriting/
│   ├── SKILL.md
│   ├── examples/
│   │   ├── winning-ctas.md
│   │   └── losing-ctas.md
│   └── version-history.md
└── persona.md                      ← Updated with proven voice patterns
```

## Key Principle

**Skills are living documents.** They evolve based on what actually works in the field, not what sounded good in theory. But evolution is slow and deliberate — 10+ data points, 20% win rate difference, Colin's approval. No knee-jerk changes.
