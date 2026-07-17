# Narrative World Model — 8/10

**arxiv:2607.05577** · Kelly Hong, Anton Troynikov, Jeff Huber · July 7, 2026  
🔗 https://arxiv.org/abs/2607.05577

---

## What it is

A typed temporal-state graph for tracking narrative state during long-form story generation. Rather than keeping the full story history in context, it stores story state as a structured graph with six node types:

- **Focalization nodes** — whose POV is currently active; tracks camera placement across scenes
- **Epistemic state nodes** — who knows what within the story world (per-character knowledge sets)
- **Event-vs-reveal order** — when events *happened* vs. when readers are *told* about them (handles flashbacks, mysteries, dramatic irony)
- **Dramatic function labels** — each scene tagged with its structural role (hook, complication, crisis, resolution, etc.)
- **Promise/payoff pairs** — tracks every narrative commitment and its eventual fulfillment
- **Validity intervals** — time-bounded facts: "Character A has the key during scenes 3-7 only"

At generation time, a query-conditioned hybrid retriever (BM25 + vector + one-hop graph expansion) fetches only the relevant subgraph for the current scene, rather than loading full history into context. This keeps per-turn context lean even across 100k-word novels.

---

## Why you'd want it (specific to your stack)

Your fiction pipeline's hardest failure modes are:

1. **Characters reveal things they couldn't know yet** — the epistemic state graph directly prevents this. Every generation query can be gated: "what does [character] know at scene N?"
2. **Promises go unfulfilled across chapters** — the promise/payoff tracker is a native implementation of what you've been doing manually in story bibles
3. **Context bloat in long sessions** — the hybrid retriever solves the "75k token session carrying chapter 1 plot state through chapter 12" problem that the context-rot briefing (AM run) named

The validity intervals node type is particularly useful for worldbuilding: "the portal is open during the solstice week" is a bounded fact that standard RAG conflates with permanent world rules.

---

## Why I think it's worth your attention

The AM run surfaced the StoryWriter paper (multi-agent story pipeline with a shared story bible) and the context-rot briefing (session degradation patterns). This paper is the structural layer that makes those patterns work at novel length: the story bible becomes a typed graph, and context rot becomes a graph-query problem rather than a context-window problem.

The epistemic-state tracking is the gap in every fiction pipeline I've seen — including the StoryWriter architecture. It's the difference between a continuity checker that knows *what happened* and one that knows *who knows what happened*.

---

## What to do

**Near-term (next pipeline iteration):**
- Model the story bible as a YAML graph with the six node types above — even a flat YAML with those keys gives you the schema without implementing graph traversal
- Add a "knowledge gate" to scene generation prompts: before drafting dialogue, query the epistemic state for each speaking character. Simple implementation: a small CC skill that accepts `character`, `scene_number`, and returns what that character knows at that point

**Medium-term (when the pipeline is stable):**
- The hybrid retriever (BM25 + vector + graph-hop) is buildable as a CC skill wrapping a lightweight local graph store (NetworkX or a SQLite adjacency table). The BM25 layer handles keyword-specific narrative facts; the vector layer handles semantic similarity; the one-hop expansion catches implicit dependencies

**Paper to read first:**
Section 3 (graph schema) and Section 5 (retriever architecture) are the buildable parts. The QA benchmarks in Section 6 are useful for calibrating how much the retrieval layer actually buys you vs. naive full-context.

🔗 https://arxiv.org/abs/2607.05577
