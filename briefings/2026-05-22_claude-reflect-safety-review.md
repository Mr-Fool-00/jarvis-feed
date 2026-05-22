# claude-reflect — Self-Learning Hook System — 8/10 (SAFETY GATE)

## What it is

A GitHub tool (1,000 stars, 88 forks, last updated February 2026) that automatically captures every time you correct Claude Code during a session, and turns those corrections into permanent memory.

It works in two stages:
- **Stage 1 (automatic):** Hooks watch your Claude Code sessions. When you correct Claude, the hook detects it (via pattern matching + AI analysis) and queues it as a potential learning with a confidence score.
- **Stage 2 (you review):** You run `/reflect` and get a table: here are the things I think I learned today. You approve, edit, or skip each one. Approved learnings sync to your `~/.claude/CLAUDE.md` and generate draft skill files in `.claude/commands/`.

A v2 feature (`/reflect-skills`) also scans your history for repeating patterns and auto-generates skill commands.

Created by: BayramAnnakov. Repo: https://github.com/BayramAnnakov/claude-reflect

## Why you'd want it

Same value as the nightly cron briefing — makes Claude smarter over time from your corrections — but with a human review gate so nothing slips through unreviewed. The table review is 2 minutes per day instead of manually editing CLAUDE.md.

## Why I think it matters

The review gate is the right design. Blind auto-learning is how you end up with a CLAUDE.md full of contradictions. This lets you see exactly what the system thinks it learned and correct it before it sticks. That's the version worth building.

## What I will do (safety rule)

I will NOT install this tool. It installs hooks into your Claude Code session lifecycle and writes to `~/.claude/CLAUDE.md` and `.claude/commands/` — that's the entire instruction layer of your Jarvis setup. Before touching any of that with third-party code, I need to read the source.

Here's what I found in my deep dive:
- **Hook architecture:** Uses Claude Code's PostToolUse and Stop hooks. Standard, legitimate use of the hooks system.
- **Files modified:** `~/.claude/CLAUDE.md` (global), `./CLAUDE.md` (project), `.claude/commands/` (new skill files). No network calls outside of Claude API.
- **Known issues:** Cache invalidation problems on plugin updates (requires manual reinstall + cache clear). Session retention dependency (relies on 30-day log retention — docs recommend extending to 99,999 days via settings.json).
- **Red flags found:** None critical. The cache issue is an operational annoyance, not a security concern. Maintainership is active.

**My recommendation:** React 🚀 → I build a Leo-owned version of this (not a copy — inspired by, with our own implementation). Ours would:
1. Use the same two-stage hook + review pattern
2. Skip the cache-dependent session scanning (use the JSONL log approach from the nightly cron briefing instead)
3. Produce a cleaner `/reflect` review format
4. Give us code we understand and can tune

Building our version sidesteps the install risk entirely and gives us something better-fitted to your setup.

🔗 https://github.com/BayramAnnakov/claude-reflect
