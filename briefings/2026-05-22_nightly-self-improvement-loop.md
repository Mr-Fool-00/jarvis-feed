# Nightly Self-Improvement Loop for Claude Code — 8/10

## What it is

A Towards Data Science article (May 15, 2026) from a practitioner who set up a cron job that runs every night and makes their Claude Code smarter by morning.

Here's how it works: Claude Code saves everything it does to JSONL log files. The nightly cron starts a new Claude session, tells it to read the last 24 hours of logs, find every time the human corrected it or something went wrong, extract the general lesson (not just "don't do X" but "don't patch widely-used infrastructure even if it seems isolated"), and append it as a new rule to `~/.claude/CLAUDE.md`.

Next morning, Claude reads that file and starts smarter. Repeated every night, it compounds. Practitioners report measurable improvement within a week.

The idea has at least 3 independent implementations converging right now: this TDS article, Addy Osmani's earlier write-up, and the `claude-reflect` GitHub tool (1,000 stars) — all describing the same core mechanic from different angles.

## Why you'd want it

Jarvis already does this at the macro level for me — `reactions.md` and `feedback.md` feed into how I rank future discoveries. This is the same idea but for YOUR Claude Code sessions during the day: writing sessions, build sessions, debugging sessions.

Right now, if Claude makes the same mistake twice across sessions, there's no mechanism to stop it. With this pattern running as a nightly Routine, each mistake trains CLAUDE.md so it won't repeat.

For your novel pipeline specifically: every time you've corrected Claude on voice, chapter structure, or worldbuilding rules — this would capture those corrections automatically and turn them into permanent session context.

## Why I think it matters

This is the cleanest compound improvement available to you right now. Zero cost, zero new tools. Just a scheduled Claude session + CLAUDE.md. The implementation is one afternoon of work and then it runs itself forever.

And unlike most "make AI smarter" ideas that require API access, new tools, or vector databases — this works entirely inside Claude Code's existing session infrastructure. It's self-contained.

## What to do

React 🚀 → I'll implement this as a Claude Code Routine (scheduled task):
- Runs nightly at midnight CDT
- Scans today's session JSONL logs
- Extracts corrections + inefficiencies
- Formats as structured CLAUDE.md rules with dedup check
- Appends to `~/.claude/CLAUDE.md` with date stamp

Estimated implementation: one session. Produces `state/nightly-reflect.md` (the routine definition) + a first run to seed your CLAUDE.md with any obvious patterns I can spot already.

🔗 https://towardsdatascience.com/how-i-continually-improve-my-claude-code/
🔗 Companion: https://towardsdatascience.com/how-to-make-claude-code-improve-from-its-mistakes/
🔗 Simple gist version: https://gist.github.com/a-c-m/f4cead5ca125d2eaad073dfd71efbcfc
