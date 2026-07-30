# GitHub Visibility Rules

## The Rule

> **If it names a real brand, business, or personal identifier — keep it private.**

## What's Public

| Content Type | Example |
|-------------|---------|
| Generic skills and frameworks | `copywriting`, `research-agent`, `context-optimizer` |
| Open-source tools | `defuddle`, `repomix` integrations |
| Public learning resources | Animation vocabularies, UI libraries |
| Generic agent workflows | Lead researcher, proposal writer (with `[Your Brand]` placeholders) |

## What's Private

| Content Type | Example |
|-------------|---------|
| Brand-specific workflows | Vitae10 supplier emails, FSA compliance |
| Personal identifiers | `colin@vitae10.com`, specific email addresses |
| Business operations | Supplier negotiations, pricing, manufacturing |
| Financial data | Budgets, revenue projections, customer data |
| Proprietary IP | Before public release, scrubbed versions can be published |

## The Scrubbing Process

Before making a private repo public:

1. **Search for brand names** — `grep -ri "vitae\|colin1miley\|your-real-brand" .`
2. **Replace with placeholders**:
   - `Vitae10` → `[Your Brand]`
   - `vitae10` → `[your-brand]`
   - `colin@vitae10.com` → `[your-email@example.com]`
   - `£39/month` → `[your price]`
3. **Verify** — Search again to confirm nothing leaked
4. **Document** — Add this SCRUBBING.md to the repo
5. **Make public** — Only after verification

## Enforcement

- All repos created with business content start as **private**
- Scrubbing required before any visibility change to public
- When in doubt: **keep it private**

## History

- **2026-07-30** — `agent-swarm` scrubbed and made public after Vitae10 references removed
- **2026-07-30** — `vitae10-private` created as private repo for all Vitae10-specific work
