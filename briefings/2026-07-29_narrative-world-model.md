# Briefing: Narrative World Model — Typed Temporal Memory for Long-Form Fiction

**Source:** arxiv:2607.05577 (PocketFM, July 6 2026) · **Score:** 8/10 · **Type:** Architecture pattern to implement

---

## The problem it solves

Your fiction pipeline has no structured memory for narratological state. The questions that break your current setup:

- Who knows secret X, and when did they learn it?
- Was this event described before or after it happened in story-time?
- Is Chekhov's gun from chapter 3 still open?
- How has the A/B relationship shifted across 22 chapters?

Flat RAG and general-purpose agent memory (Graphiti/Zep, GraphRAG) can't answer these. They track entities and facts, not narrative structure. The NWM paper formalizes exactly what you need.

---

## The architecture (two layers)

**Layer 1: Typed temporal-state graph**

A knowledge graph where every node/edge carries chapter-indexed validity intervals. Not just "X is true" — "X is true from chapter 7 to chapter 14; after chapter 15, Y." Six typed record families extracted from each finalized chapter:

| Record type | What it tracks |
|---|---|
| Focalized observer | Which character's perception renders a scene |
| Epistemic state | Per-character knowledge boundary at each checkpoint |
| Event-vs-reveal order | When events happened (story-time) vs. when narrated (discourse-time) |
| Dramatic function | Scene beat labels: setup / complication / turning point / resolution |
| Promise/payoff | Open/closed status on each planted narrative promise |
| Relationship delta | Directional relationship change, timestamped to chapter |

**Layer 2: Chapter-safe evidence packets**

When retrieving context for chapter N, the retrieval system (BM25 + vector + one-hop graph expansion) only surfaces records valid through chapter N. Records from chapter N+1+ cannot bleed in. Prevents consistency breaks where retrieved context reveals what the character (or writer) isn't supposed to know yet.

---

## Results

Beats Graphiti/Zep 0.709 vs. 0.582 on 110-question multi-hop narratological QA (p = 0.054). The gain is structural — holds even when Graphiti is rebuilt with NWM's own extractor.

---

## What to do

1. **Read the paper** — https://arxiv.org/abs/2607.05577. Focus on the extraction schema (typed record definitions) and Section 4 (chapter-safe retrieval). Skim the ablations for which record types matter most.

2. **Check for code release** — No confirmed public GitHub URL yet (preprint, submitted July 6). Watch the PocketFM GitHub org. The paper states "code and data available at respective repositories."

3. **Prototype the extraction step** — The core implementation is: after each chapter finalizes, run a Claude Sonnet extraction call that outputs the six record types as structured JSON, then append to a temporal graph file. This is a ~2-hour prototype.

4. **Use the eval harness** — The benchmark questions are reproducible on Project Gutenberg books with a held-constant Claude reader. If you build this, you can measure it against their baseline.

---

## Why now

You're building chapter pipelines. This paper describes exactly the memory layer that scales — the same patterns that let PocketFM manage production serialized fiction at 50+ chapters. The typed record schema is the hardest intellectual part; they've done it. The implementation is the straightforward part.

🔗 https://arxiv.org/abs/2607.05577
