# Claude Code Routines — 9/10

## What it is
A first-party Anthropic feature (research preview since April 14) that lets you define a prompt + repo + connectors, then run it automatically on a schedule, via API call, or on GitHub events — all on Anthropic's cloud. Your laptop doesn't need to be open. Max plan gets 15 runs/day.

## Why you'd want it (specific to your stack)
Your Jarvis system currently runs: launchd daemon on your Mac → polls Cloudflare KV every 5s → spawns `claude -p` locally → POSTs result back to Worker. That's 4 moving parts for what Routines does natively in 1. The Discovery Loop (this very run), morning briefs, wiki refreshes, and any other scheduled Max task could become a Routine: define the prompt once, attach the repo, set a 12h/daily/weekly schedule, done. No daemon, no plist, no heartbeat monitoring, no local Mac dependency.

## Why I think it's worth your attention
This is Anthropic building the thing you built yourself. Your daemon works, but Routines removes the entire daemon codebase + launchd config + watchdog workflow for scheduled work. Real-time Slack DM response still needs the daemon (Routines are scheduled, not event-stream), but the heavy cron jobs are perfect migration candidates.

## What to do
Try it: go to claude.ai/code/routines and create a test routine with a simple prompt against jarvis-feed. Set a one-time schedule. If it works, the migration plan is:
1. Discovery Loop → Routine (12h schedule, AGENT_RUNBOOK.md as prompt)
2. Morning brief → Routine (daily 7am CDT)
3. Wiki refresh → Routine (weekly)
4. Keep daemon ONLY for real-time Slack DM → KV → response flow

🔗 https://code.claude.com/docs/en/routines
