# VERDICT: SKIP — Claude Code Routines

## Why skipped

Claude Code Routines is a **first-party Anthropic platform feature** — scheduled cloud execution of Claude Code prompts against a repo. It's accessed via claude.ai/code/routines, not installed or built locally.

There is nothing to build as a skill or slash command here. The briefing's action item is "go to claude.ai and create a Routine" — a platform migration decision, not a buildable artifact. You can't create a "native version" of someone else's cloud infrastructure.

## What it actually is

- Research preview since April 2026
- Runs Claude Code prompts on Anthropic's cloud on a schedule (cron), via API, or on GitHub events
- Max plan: 15 runs/day
- Potential migration target for Leo's daemon-based scheduled tasks (Discovery Loop, morning briefs, wiki refreshes)

## Relevance to Leo's stack

High relevance as a **migration opportunity**, but that's an ops decision Leo makes manually, not something Max builds. The daemon still handles real-time Slack DM flow (Routines are scheduled, not event-driven).

## Recommendation

Leo should evaluate Routines when he's ready to simplify the daemon. Not a skill-slot candidate.
