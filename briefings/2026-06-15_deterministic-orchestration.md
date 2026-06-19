# Deterministic Orchestration Pattern: 130-Line Script, 9 Parallel Agents — 7/10

**Date:** 2026-06-15
**Source URL:** https://alexop.dev/posts/claude-code-workflows-deterministic-orchestration/
**Score:** 7/10
**Category:** Practitioner pattern for Dynamic Workflows (not third-party code to install — design pattern only)

---

## What it is

A developer built a 130-line TypeScript/JavaScript Dynamic Workflow script that does this: define 9 different search tasks, spawn all 9 as parallel Claude agents, wait for them all to finish, collect their outputs into one list, rank by importance, write a final digest. The whole thing is automated — no human clicks between steps.

The key design principle: **the script's control flow is 100% code. Only what happens inside each agent() call is Claude.** So the "spin up 9 agents in parallel" decision is made by code (deterministic). The actual research + writing each agent does is made by Claude (probabilistic). This is the clean separation that makes the system reliable.

It also validates output schemas — so if an agent returns garbage, the script catches it before it corrupts downstream steps.

---

## Why you'd want it (specific to your stack)

This is exactly what your book pipeline needs for parallel chapter generation.

Right now: you probably run chapters sequentially. Chapter 1 → Chapter 2 → Chapter 3.

With this pattern: you run Chapter 1, 2, 3... N all at once in parallel. Each agent gets its own context (world bible, previous chapter handoff), writes its chapter, validates the output, and saves the handoff file. The orchestrator waits for all chapters in an arc to complete before moving to the next arc.

The concrete mapping:
- `agent(prompt, {schema: chapterOutputSchema})` → one chapter-writing agent with enforced structure
- `parallel([chap1, chap2, chap3])` → all three run simultaneously
- Schema validation → catches "agent wrote 0 words" or "agent hallucinated character name" before it pollutes the pipeline

This cuts your wall-clock time from (N chapters × minutes per chapter) to (max chapter time + coordination overhead). That's the 12-hour novel wall time.

---

## Why I think it's worth your attention

The alexop.dev article is showing you the exact same architecture that Jarvis uses for its discovery loop — 9 parallel source searches, collected and ranked. It works. It's a 130-line script, not a complex framework. You can read the whole thing in 10 minutes and understand how to adapt it for chapters instead of news sources.

---

## What to do

This is not a third-party package to install — it's a pattern to read and adapt. The Dynamic Workflows feature is already in Claude Code. You'd write your own `/book-pipeline.js` workflow script using the same `agent()` + `parallel()` primitives.

1. Read the alexop.dev article (link below) — pay attention to the schema validation section specifically.
2. Decide if you want me to draft a `/book-pipeline.js` workflow script using this pattern for your fiction pipeline. If yes, I build it next run.
3. The Trilogy AI explainer (https://trilogyai.substack.com/p/claude-codes-dynamic-workflows-a) is a good companion read if you want the theory before the code.

🔗 https://alexop.dev/posts/claude-code-workflows-deterministic-orchestration/
