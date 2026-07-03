# Briefing: From Personas to Plot — Multi-Agent World-State Story Generation

**Date:** 2026-07-03
**Score:** 8/10
**Paper:** arXiv:2607.00918 (submitted July 1, 2026)
**Authors:** Pocket FM, Princeton, University of Michigan, University of Maryland, Universitat Pompeu Fabra
**Verdict:** BUILD-WORTHY — this is the architecture blueprint for a hallucination-resistant novel-length generation pipeline

---

## The problem it solves

When you ask a single LLM to write a long story — 50, 100 pages — it drifts. Characters forget what they did two chapters ago. Dead characters speak. Two people are in two places simultaneously. The model is coherent sentence-to-sentence but loses narrative coherence at scale.

This paper builds a system that reduces hallucinations by **50%** and annotation inconsistencies by **41%** at 100 pages, compared to single-model generation. It also beats the previous best multi-agent story system (IBSEN) by 45% and 34% respectively.

No novel model training required. Works with existing LLMs as agents.

---

## The architecture: MAGNET + ATLAS

### MAGNET — the generation engine

Each character in the story is a **separate agent** with a fixed persona. The agents don't share a single context window — they share a **world state**: a structured record of everything that has happened in the story so far.

When it's a character's turn to act, that agent:
1. Reads its own persona (who they are)
2. Reads the current world state (what has happened)
3. Reads the current story goals (what the narrative is trying to accomplish)
4. Proposes an action

The action gets validated against the world state before it becomes part of the story. If the proposed action contradicts what's already happened (a character who died in chapter 3 can't make a proposal), it gets rejected.

This is real multi-agent interaction — agents constrain each other through the world state, not through a single monolithic prompt.

### ATLAS — the consistency checker

ATLAS runs *after* MAGNET generates content. It builds a graph of each scene's world state — who is where, what is established, what has changed — and compares graphs scene-to-scene.

If something contradicts (character in two places, fact established in scene 4 violated in scene 7), ATLAS flags it.

**Key insight:** ATLAS is separable. It doesn't need to run with MAGNET. You can run ATLAS over any existing long-form generated text as a standalone consistency audit.

---

## What you can actually build with this

**Option A: Full MAGNET pipeline**
- Assign a Claude agent to each major character in Leo's fiction project
- Maintain a shared JSON world state that agents read and write after each chapter
- Validate proposed actions against the world state before committing them
- Run ATLAS-style consistency checks across scene graph deltas at chapter boundaries

This is significant implementation work — roughly a fiction-pipeline architecture rework. But the 50% hallucination reduction at 100 pages is a compelling target for overnight novel generation.

**Option B: ATLAS-only consistency checker (lower effort)**
- After each Jarvis overnight run generates chapter content, run a consistency-checking pass over the chapter output
- Build a lightweight world-state graph from the generated text (character locations, established facts, timeline)
- Compare against the accumulated graph from previous chapters
- Flag contradictions for Leo's review

This is the lower-risk path: bolt ATLAS on top of whatever pipeline already exists. The generation engine doesn't change; only a verification pass is added.

---

## Recommendation

Start with Option B. Implementing a scene-level world-state consistency checker as a post-generation audit step is tractable in an afternoon and immediately improves the quality signal from overnight fiction runs. Option A is the full architecture upgrade — worth a separate project brief when Leo is ready to rearchitect the pipeline.

🔗 https://arxiv.org/abs/2607.00918
