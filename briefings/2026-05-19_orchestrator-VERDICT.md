# Orchestrator paper — VERDICT: SKIP

**Date:** 2026-05-19
**Source briefing:** `2026-05-19_rl-multi-agent-orchestration-paper.md`
**Decision:** Do NOT build `/orchestrate`.
**Re-reviewed:** 2026-05-20 — verdict stands. Taxonomy useful for Council v2 improvements, not a new command.

## TL;DR

The paper contributes a **diagnostic taxonomy** (5 decisions, 8 reward signals) for multi-agent orchestration. It does NOT contribute a new workflow shape. The taxonomy's value is improving the existing `/council` skill, not standing up a fourth parallel-agent command.

The briefing itself ends with: *"save it as a reference for the next major Council skill revision post-finals."* That's the correct read. Author didn't recommend building anything; they recommended using the vocabulary to diagnose Council failures.

## Overlap audit

A hypothetical `/orchestrate` command (parallel-agent decomposition of complex tasks) would overlap heavily with three existing assets:

| Asset | What it already does | Overlap with /orchestrate |
|---|---|---|
| `/council` | Spawns 5 fixed advisors in parallel, peer review, synthesis | Decision 1 (when to spawn), 2 (whom), 4 (aggregate) all baked in |
| `/research` | Decomposes a question into 20-50 atomic tasks, executes, adversary verification, synthesizes | Decision 1, 3 (communicate), 4, 5 (when to stop) all baked in |
| `superpowers:dispatching-parallel-agents` skill | Auto-triggers when 2+ independent tasks can be parallelized | This IS the orchestration primitive — every other workflow leans on it |

There is no shape of "complex task decomposition + parallel dispatch + aggregation" that isn't already covered by one of the three. A new `/orchestrate` would be either a worse `/council`, a worse `/research`, or a renaming of an existing skill.

## What the paper IS good for

The 5-decision taxonomy is a **diagnostic lens**, not a workflow:

1. **When to spawn** — currently `/council` spawns 5 advisors unconditionally. Could become "spawn 2 for narrow questions, 5 for ambiguous ones, 7+ for strategy."
2. **Whom to delegate** — `/council`'s 5 personas are fixed. Could become "select advisors from a pool based on query classification."
3. **How to communicate** — each advisor currently gets the raw user question. Could become "Chairman briefs each advisor differently based on their lens."
4. **How to aggregate** — synthesis is currently a single Chairman pass. Could become weighted by advisor confidence + counter-quote weight (mirroring /research's adversary logic).
5. **When to stop** — Council is one-shot. Could become "ask Chairman: do we need a second round? if yes, which advisors?"

These are five concrete Council v2 improvements. They are NOT a new command.

## Recommendation

When Leo revisits `/council` post-finals (per briefing's suggestion):
- Use the 5-decision frame to audit current Council weaknesses
- Pick the 2-3 decisions that most often go wrong in practice
- Patch those into Council, don't build a new thing

Until then: no action required. The briefing is correctly filed as "reference for future iteration," not "actionable now."

## File status

- `/Users/leograu/.claude/commands/orchestrate.md` — NOT CREATED (intentionally)
- This VERDICT file is the closure record
