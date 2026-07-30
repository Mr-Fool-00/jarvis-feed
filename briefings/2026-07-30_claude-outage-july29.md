# Claude Global Outage — July 29, 2026 — 7/10

## What it is
A platform capacity failure hit Claude worldwide between 19:45–21:26 UTC on July 29, 2026 — roughly 3:45–9:26 PM CDT. Users saw HTTP 529 "Model Overloaded" errors (not 429 rate limiting — 529 means the platform itself choked). About 5,000+ users were affected, with Downdetector showing roughly half the reports coming specifically from Claude Code users.

## Why you'd want it (specific to your stack)
If any of your book pipeline sessions or long-running CC tasks were running between 7 PM and 9 PM CDT last night, the outage is what killed them — not a bug in your code. Anthropic confirmed recovery at ~21:26 UTC. No recurrence signals since.

## Why I think it's worth your attention
529 vs 429 matters: 529 is infrastructure failure, not your rate limit — retry logic alone won't save you, you need to detect the specific code and back off intelligently.

## What to do
Note: if you build retry logic into long-running pipeline sessions, detect 529 separately from 429 and use exponential backoff with a longer ceiling (30–60 min, not seconds). Could be worth a runbook rule or a skill later if this keeps happening.

🔗 https://www.bleepingcomputer.com/news/artificial-intelligence/anthropic-confirms-claude-is-down-worldwide/
🔗 https://www.newsweek.com/claude-down-outage-capacity-constraints-not-working-anthropic-12262120
🔗 https://glitchwire.com/news/anthropics-claude-goes-down-for-thousands-as-529-errors-hit-workers-mid-task/
