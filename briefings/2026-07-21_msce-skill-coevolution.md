# Briefing: MSCE — Memory and Skills Co-Evolution for Jarvis Self-Improvement

**Date:** 2026-07-21 PM
**Score:** 8/10 · **Paper:** arXiv:2607.16621 (July 18, 2026) · **build_worthy=TRUE**
**URL:** https://arxiv.org/abs/2607.16621
**Tag:** skill-evolution, self-improvement, agent-traces, training-free, build-worthy

---

## What the paper proposes

MSCE (Memory and Skills Co-Evolution) is a training-free framework for agent self-improvement that treats **successful execution traces as the raw material for new skills.** The loop:

1. Agent runs a task
2. If the task succeeds with a high-quality multi-step trace, MSCE extracts the reusable structure from the trace
3. The extracted structure is stored as a callable skill, linked to the evidence trace that produced it
4. Skills are rated by evidence count (how many successful traces have invoked this pattern) and recency (recent evidence outweighs stale evidence)
5. Low-evidence skills decay and are pruned
6. On the next run, the agent has access to the skill library; it selects relevant skills as shortcuts for similar task types

**Why it's training-free:** No fine-tuning, no gradient descent. The "learning" is purely retrieval and pattern matching — successful traces are indexed, future runs retrieve similar traces, and the agent uses the retrieved trace structure as a scaffold for the new task. Works with any LLM that can follow a retrieved pattern.

**Benchmarks:** 6 tasks across coding, planning, and information retrieval domains. Performance gain over baseline (no skill library): 15–34% depending on task type. Gain over "static skill library" (pre-written skills that don't evolve): 8–19%. The evolution matters, not just having any skill library.

---

## Why this is directly buildable for Jarvis

Jarvis already produces evidence: every digest run is a structured trace of decisions — which sources produced signal, how items were scored, which items were truly-want, which builds were validated. That trace currently disappears after each run.

MSCE formalizes exactly what's missing: **a feedback loop where Jarvis's own high-quality reasoning becomes a future skill input.**

### Concrete Jarvis implementation

**Capture phase (new, post-run):**
After each digest run, Jarvis runs a "trace extraction" pass over the session:
- For each item scored 8+/10: extract the source chain (how it was found, what made it high-signal)
- For each build_worthy=TRUE item: extract the build verdict reasoning as a skill fragment
- For each truly-want=TRUE item: extract what triggered the tap-on-shoulder judgment

These get stored as small structured fragments in `state/skill-traces/YYYY-MM-DD_<slug>.json`.

**Retrieval phase (existing run, pre-ranking):**
When Jarvis scores new items, it retrieves the N most similar skill-trace fragments (by topic/source/pattern similarity) and uses them as ranking calibration context. Example: "The last time Jarvis saw an MCP security story, it scored 7 and triggered truly-want=TRUE. The current item is similar in structure — adjust score accordingly."

**Evidence gating:**
- A skill fragment with 1 trace is flagged as "low-evidence" — used with low confidence weight
- A fragment with 3+ traces becomes "stable" — used at full weight
- Fragments older than 30 days without new evidence decay by 50% weight
- Fragments older than 90 days are pruned

---

## Implementation scope

Minimal implementation (Phase 1, no new external dependencies):
- Add a `state/skill-traces/` directory
- After each run, write structured JSON fragments for high-signal items
- Before ranking, read recent fragments as context in the ranking prompt

The traces are already produced — they just need to be persisted. No new model calls required; the extraction can be done in the same ranking-pass prompt.

Estimated build time: ~2 hours to add the capture phase; ~1 hour to wire the retrieval phase into the ranking step. Leo to review approach before implementation.

---

## What the paper found that's surprising

The **recency weighting** is the most counterintuitive finding: evidence from the last 7 runs outperforms a larger evidence pool spanning 30 runs when the domain is fast-moving. For AI news digests, where what "high-signal" means changes week to week (e.g., "security MCP items" became important this week, wasn't last month), this suggests keeping the retrieval window short — last 7-10 runs — rather than accumulating all historical traces.

**Implication for Jarvis:** Don't accumulate skill-traces indefinitely. Keep the active skill library as the last 10 runs' worth of high-signal items. Archive the rest to `state/skill-traces/archive/` but don't retrieve from archive during normal runs.

---

## Suggested briefing action

- [ ] Leo approves the concept (trace extraction + retrieval loop)
- [ ] Jarvis builds Phase 1: add `state/skill-traces/` capture after next run, no retrieval yet (validate the trace quality first)
- [ ] Jarvis builds Phase 2: wire retrieval into ranking step, tune evidence weights over 2 weeks of runs
- [ ] After 14 days: compare scoring calibration against pre-MSCE baseline (Leo's truly-want accuracy is the signal)
