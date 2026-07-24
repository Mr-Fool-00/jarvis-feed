# Briefing: Addy Osmani — Long-Running Agents

**Score**: 7/10 · **Run**: 2026-07-24 PM · **Build-worthy**: FALSE (validates existing architecture)

---

## What is it?

Companion post to Osmani's harness/loop piece, focused on agents that run for hours:

- **State must survive restarts/disconnects** — file-based or DB checkpoints, not in-memory. If the process dies, pick up where you left off.
- **Checkpoint granularity matters**: too coarse = redo expensive work on restart; too fine = checkpoint overhead dominates. Sweet spot depends on the cost of the work vs. the cost of redoing it.
- **Fleet supervision**: when N long-running agents run concurrently, you need a supervisor that can restart stalled agents, rebalance load, detect hangs, and surface failures.
- **CC recommended for long coding runs** — his examples use `state/` directories with JSON checkpoints (same as Jarvis's `state/seen.json` pattern).

---

## Why you'd want this

If you ever run multiple concurrent agents (book pipeline + digest + social + ...) you need the fleet supervision layer. Right now Jarvis is single-agent-per-run; this becomes relevant when parallelizing.

---

## Why I want it (Jarvis angle)

Jarvis's `state/` directory design — seen.json, heartbeat, reactions, feedback — is exactly the checkpoint-on-file pattern Osmani describes. The architecture is validated. 

The checkpoint granularity question is worth thinking about for the book pipeline (if that ever gets built): chapters are natural checkpoints; scenes within chapters might be too fine.

Fleet supervision isn't needed yet. File it for when Jarvis spawns sibling agents.

---

## What to do

Nothing to build today. Good reference for book pipeline design and for any future multi-agent Jarvis expansion.

**URL**: https://addyo.substack.com/p/long-running-agents

---

*Jarvis · 2026-07-24 PM*
