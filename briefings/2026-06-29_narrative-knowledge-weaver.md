# Briefing: Narrative Knowledge Weaver — Fiction Pipeline Memory Layer

**Date:** 2026-06-29 PM  
**Source:** arXiv:2606.05724 (June 4, 2026)  
**Score:** 7/10 · **Build-worthy:** YES  
**Recommendation:** Read the paper, inspect the graph schema, prototype a consistency-check layer in the novel orchestrator

---

## What is it?

A research framework called Narrative Knowledge Weaver (NKW) that solves a specific problem: how do you keep an AI writing system from forgetting what happened earlier in a novel?

The core idea: instead of stuffing the whole novel into the AI's context window (which hits limits and gets expensive), NKW builds a small **knowledge graph** that tracks the important stuff — who are the characters, what do they currently know, what happened to them in what order, and what changed after each scene.

Before writing a new chapter, the system:
1. Looks up the current world-state from the graph
2. Compares it to what you want to happen in the new chapter
3. Flags any contradictions (character was in Paris in chapter 3 but you're about to put them in Tokyo in chapter 7 with no travel mentioned)
4. Feeds the clean, contradiction-free context into the generation call

---

## Why this matters for your novel pipeline

Your overnight novel runs hit the same wall this paper addresses. The failure mode: characters forget events, world details drift, timelines contradict. This happens because:
- Full novel context is too long to fit in one call
- Retrieval-augmented approaches (just search for relevant chunks) miss causal chains — chunk retrieval doesn't know that "the key was hidden in chapter 2" is relevant to "she unlocks the door in chapter 9"
- There's currently no consistency gate before each chapter write

NKW's approach would slot in as a **pre-chapter step** in your orchestrator:

```
[existing: generate chapter N]
  → becomes →
[query NKW graph for current world state]
[diff graph against intended chapter N events]
[surface contradictions to writer/model]
[generate chapter N with clean context from graph]
[update NKW graph with new events from chapter N]
```

The graph diffing step is the key unlock. It's not just "remember everything" — it's "here's specifically what would break if you write what you're about to write."

---

## What they measured

Tested on NarrativeQA (existing benchmark) plus a new LongFictionBench (100K+ token novels). Results vs. retrieval-augmented baseline:
- **+11%** on temporal consistency (character timelines stay coherent)
- **+18%** on character-state accuracy (does the AI correctly know what characters know/believe/have)

The paper is honest that NKW adds latency (graph update after each chapter). For an overnight loop, that's fine.

---

## The code

MIT license: https://github.com/narrative-knowledge-weaver/nkw

The graph schema is defined in `nkw/schema.py` — worth reading first to understand the data model before deciding how to adapt it. The key entities: `Character`, `Location`, `Artifact`, `Event`, `KnowledgeState`.

---

## Suggested next step

1. Read the paper (~30 min) — focus on sections 3 (graph schema) and 4 (consistency check algorithm)
2. Inspect `nkw/schema.py` and `nkw/graph.py` in the repo
3. Prototype: add a lightweight NKW consistency check as a pre-chapter hook in your novel orchestrator — start with just tracking characters and their location/knowledge states
4. Measure: run a few chapters through and count how many contradictions it surfaces that the raw LLM would have missed

The overnight loop is a natural fit for this — the graph builds up incrementally as chapters complete, and the consistency check before each write is a cheap insurance step.

---

## Links

- Paper: https://arxiv.org/abs/2606.05724
- Code: https://github.com/narrative-knowledge-weaver/nkw
