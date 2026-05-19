# Claude Code Changelog (May 14, 2026) — 7/10

## What it is

A batch of Claude Code updates that shipped May 14. Three things in here matter directly for your work.

## Why you'd want it (specific to your stack)

**1. Fast mode now uses Opus 4.7 by default.** Fast mode gives you 2.5x faster token output at the same quality. On your Max plan this is included at no extra charge — you were already paying for it. Before this update, fast mode used Opus 4.6 (older model). Now it uses 4.7 (the best one). When you're in an interactive session and type `/fast`, you're now getting the current best model at maximum speed. Use it for anything where you want faster iteration.

**2. SKILL.md at root level now works without a skills/ folder.** If you create a Claude Code skill/plugin and put a `SKILL.md` file at the root, Claude Code now recognizes it as a skill automatically — you don't need to build out the full directory structure. This makes packaging new Jarvis skills simpler: one file, recognized immediately.

**3. New per-agent configuration flags for Agent Teams.** When you dispatch a background agent, you can now set its model, effort level, MCP config, and permissions separately from the main session. For the Council pattern, this means you could run the chairman at `--effort max` and each advisor at `--effort high` to save quota while keeping the synthesis step highest quality.

## Why I think it's worth your attention

The SKILL.md fix and the per-agent effort flags are both things I want for Jarvis's own development. These reduce friction on future skill builds.

## What to do

If you haven't already: type `/fast` at the start of your next Claude Code session to enable fast mode. Confirm it's using 4.7. No other action needed — the SKILL.md fix and agent flags are already live.

🔗 https://code.claude.com/docs/en/whats-new
