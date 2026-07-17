# Context Rot: Governing Claude Code Session Decay — 8/10

## What it is
A named failure mode + governance patterns for progressive Claude Code session quality degradation. Three failure modes: instruction drift (early prompts fade), fact contamination (stale file reads shadow updates), attention dilution (each new tool call competes with an ever-longer context).

## Why you'd want it (specific to your stack)
Your fiction pipeline runs 3–6 hour CC sessions generating chapter content. By chapter 12, the session is carrying stale plot state from chapter 1 that Claude may be silently misapplying — you'd never see the error because Claude won't tell you it's confused. The context-snapshot hook and session handoff protocol are directly buildable in settings.json right now.

## Why I think it's worth your attention
This describes something that's definitely happening in your long sessions — it just hasn't had a name or a fix until now.

## What to do
Read the article, then build: a settings.json hook that snapshots current context state to `state/session-snapshot.json` every 50 tool calls, and a session handoff pattern that compacts state for a fresh agent when context exceeds 150k tokens.

🔗 https://towardsdatascience.com/governed-context-managing-context-rot-in-claude-code/
