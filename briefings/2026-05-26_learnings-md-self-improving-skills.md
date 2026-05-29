# Learnings.md — Self-Improving Claude Code Skills — 7/10

## What it is

A simple pattern: add a file called `LEARNINGS.md` to any Claude Code skill. Claude reads it at the start of every task, then adds new lessons to it after each run. Next time you run the skill, it starts knowing everything it learned last time. The file builds up over time — what approaches worked, what caused failures, what quirks are specific to your project. MindStudio published a 5-part guide on this (they call it the "Learnings Loop").

## Why you'd want it (specific to your stack)

Jarvis already has `state/feedback.md` and `state/agent_suggestions.md` for system-level learning. But each individual skill starts fresh every time it runs — it doesn't remember that you corrected it last session, or that a certain approach always fails on your setup.

Adding `LEARNINGS.md` to your writing skills means a skill like `write-chapter` would remember "Leo's action scenes: short bursts, under 150 words per paragraph, no long introspective pauses." Or a `voice-check` skill would remember "Chapter 3 re-read: Leo uses 'suddenly' too often, flag it." That craft knowledge accumulates automatically instead of you re-briefing Claude every session.

## Why I think it's worth your attention

It takes about 10 minutes to set up and the ROI compounds with every run. Jarvis is supposed to get smarter over time — this is the per-skill version of that. The system-level feedback already exists; this fills in the skill-level gap.

## What to do

This is a pattern (not third-party code), so no safety gate needed. You can implement it right now:

1. Create `LEARNINGS.md` in any skill folder (e.g., `~/.claude/skills/write-chapter/LEARNINGS.md`)
2. Add to your SKILL.md frontmatter: "Read LEARNINGS.md at session start. Append 2-3 key learnings after each task."

That's it. React 👍 if you want me to add LEARNINGS.md to your current Jarvis skills automatically. React 👎 to skip.

🔗 https://www.mindstudio.ai/blog/self-learning-claude-code-skill-learnings-md
