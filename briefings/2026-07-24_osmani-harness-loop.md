# Briefing: Addy Osmani — Harness vs. Loop Engineering

**Score**: 7/10 · **Run**: 2026-07-24 PM · **Build-worthy**: FALSE (vocabulary/framework, not code)

---

## What is it?

Osmani's cleanest conceptual post this month: a two-level vocabulary for pipeline architecture.

**Harness** = the agent's immediate environment:
- Which model runs
- Which tools are available
- What permissions it has
- What memory/context it gets
- What system prompt frames it

**Loop** = the system that prompts the harness, one floor above:
- Scheduling (when does it run?)
- State management (what does it remember across runs?)
- Escalation (when does it notify a human?)
- Feedback (what does it learn from reactions?)
- Improvement (when does it change?)

His thesis: the harness is increasingly commodity (Claude Code handles it); the value is in the loop.

---

## Why you'd want this vocabulary

"Own the outer loop" is the frame. When you're evaluating whether to use a new tool, ask: is this harness or loop? Buying a better harness (new model, new MCP server) is table stakes. Building a better loop (smarter scheduling, better feedback integration, escalation logic) is defensible work.

---

## Why I want it (Jarvis angle)

Jarvis IS a loop. The harness is CC + MCP tools. Jarvis's value is in the loop layer: the 12-hour cadence, the seen.json dedupe, the reaction feedback, the scoring rubric, the build-worthy filter, the briefing escalation. That's all loop engineering.

When someone asks "what does Jarvis do?" — "it's a loop that drives the Claude Code harness" is now the right answer.

Future improvement ideas are almost all loop-level: smarter reaction feedback integration, adaptive cadence, cross-run learning. Not harness-level.

---

## What to do

Apply the frame. Next time you design a new Jarvis feature, ask which layer it belongs to. No code to write today.

**URL**: https://addyo.substack.com/p/loop-harness-engineering

---

*Jarvis · 2026-07-24 PM*
