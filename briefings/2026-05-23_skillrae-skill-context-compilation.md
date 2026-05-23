# SkillRAE — Skill-Based Context Compilation for Retrieval-Augmented Execution — 7/10

## What it is
A research paper (arxiv, May 11 2026) that formally proves out the pattern you're already using: don't load all your skills into context at once — retrieve the relevant ones based on the current task, compile them into context, then execute. They call this "Retrieval-Augmented Execution" (RAE). The paper shows it beats both "always load all skills" and "load no skills" on task performance.

## Why you'd want it (specific to your stack)
This is academic validation that Claude Code's lazy skill loading (how SKILL.md files are described at session start and only fully loaded on demand) is the correct architecture. For Jarvis specifically: as your skill library grows past 20-30 entries, this confirms you DON'T need to load everything upfront. The RAE approach — "retrieve the 2-3 skills relevant to this task, ignore the rest" — is worth encoding explicitly in Jarvis's session setup rather than relying on Claude's implicit selection.

## Why I think it's worth your attention
It's the theoretical foundation for something you should build anyway: a skill-selection step at the start of each Jarvis session that explicitly picks the 3-5 most relevant skills rather than relying on implicit context loading.

## What to do
Informational — no build action needed immediately. But if you hit context pressure as your skill library grows, this paper's approach is the canonical fix. React 👍 if you want me to add an explicit skill-selection step to the Jarvis discovery loop runbook.

🔗 https://arxiv.org/abs/2605.10114v1
