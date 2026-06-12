# Anthropic Official Prompting Guide for Claude Fable 5 — 9/10

## What it is

Anthropic published an official step-by-step guide specifically for prompting Claude Fable 5. It's not a marketing page — it's a list of actual copy-paste prompts you put in your CLAUDE.md or system prompts to control how the model behaves on long, complex tasks. Has sections for: how to stop it from over-explaining, how to make it actually verify its own progress, how to run parallel sub-agents properly, how to build a simple memory system for it, and how to prevent it from stopping mid-task to ask permission.

## Why you'd want it (specific to your stack)

Your book pipeline runs long sessions where Claude generates chapters, delegates to sub-agents, and needs to report back accurately on what actually got done vs. what it just planned to do. The "audit progress against tool results before reporting" prompt directly fixes the fabricated status report problem. The parallel sub-agents section specifically says to prefer async over blocking — which is the pattern your chapter-writing pipeline needs so Chapter 3 and Chapter 4 can run simultaneously without one waiting on the other. The memory construction pattern (one lesson per file, one-line summary at top) could be pasted into Jarvis's memory system today with no code changes. The migration warning matters: your existing Opus 4.8 skills are probably too detailed for Fable 5 — you may want to simplify them before testing.

## Why I think it's worth your attention

This is the "here's how to actually use the new model" doc that comes out after every big Anthropic release, and it's usually the most actionable thing they publish. The 10-day free window on Fable 5 is your testing window. This guide is how you run that test effectively without wasting budget on misconfigured prompts.

## What to do

Open the page, read the "Parallel subagents" and "Construct a memory system" sections first — those apply directly to your pipeline today. Then read "Strong instruction following" and "Ground progress claims" for the book writing case specifically. The `send_to_user` tool definition at the bottom is worth building if you run overnight chapter agents.

🔗 https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5
