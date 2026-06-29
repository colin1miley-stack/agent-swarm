# Agent Swarm

AI agent training repository for multi-platform social media automation.

## What This Is

A framework for training AI agents to autonomously manage social media content across LinkedIn, Twitter/X, Instagram, Facebook, and TikTok. Built for wellness/tech founders building in public.

## Architecture

- agents/ — Agent personas and system prompts
- workflows/ — Task definitions and SOPs
- content/ — Content templates and brand voice docs
- knowledge/ — Domain knowledge and research
- skills-index/ — Skill-to-agent mapping
- scripts/ — Automation scripts

## Agents

| Agent | Platform | Frequency | Automation Level |
|-------|----------|-----------|-----------------|
| LinkedIn Content Agent | LinkedIn | 3x/week | Draft + Approval |
| Twitter/X Content Agent | Twitter/X | 5x/week | Draft + Approval |
| Instagram Content Agent | Instagram | 7x/week | Full Auto (feed) |
| Analytics Agent | All | Weekly | Review Reports |
| Competitor Monitor Agent | All | Daily | Auto Briefs |

## Quick Start

1. Clone this repo
2. Customize agents/linkedin-content-agent.md with your voice
3. Add your brand details to content/brand-voice.md
4. Run scripts/content-generator.py to generate your first week

## License

MIT