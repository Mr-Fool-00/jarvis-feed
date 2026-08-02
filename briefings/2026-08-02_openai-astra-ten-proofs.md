# Briefing: OpenAI Astra Stealth Reveal — Ten Math Proofs, $2K Total, Multi-Agent Root Architecture

**Date:** 2026-08-02 · **Score:** 8/10 · **Build verdict:** INFORMATIONAL
**Source:** openai.com/index/ten-advances-in-mathematics · github.com/openai/ten-proofs (Apache-2.0)

---

## What happened

OpenAI published a blog post titled "Ten advances in mathematics" on August 1, 2026. The headline is the math. The subtext is the model announcement. The model that solved all ten problems is **Astra** — OpenAI's unreleased next major model family, with no settled commercial name (not yet GPT-6 or any specific number), no release date, and no API access. This is the first public sighting of Astra.

Gizmodo's framing: *"OpenAI Smuggled the Announcement of Astra, Its Next AI Model, Into a Blog Post About Math."*

## The architecture

Astra uses a **multi-agent setup**: a root agent spawns subagents, distributes sub-problems, waits on partial results, and synthesizes the final answer. Sessions ran **over hours or days** per problem. Human researchers were not domain experts in the math — they were project managers who set goals and verified outputs. The AI did all the mathematical reasoning.

This is the same human-as-project-manager architecture that Claude used for the cryptographic weaknesses paper (Aug 1 PM digest). Two different labs. Same structure. Two days apart.

## The cost

~**$2,000 total for all ten proofs combined** — expressed in current Sol API equivalent rates. That's roughly $200 per solved-open-mathematical-problem. This is not a per-problem figure; it's the entire run.

## The ten problems (openai/ten-proofs GitHub repo)

1. **Non-sofic Groups** — First explicit construction of a non-sofic group, resolving a central open question since Gromov introduced soficity in 1999. This is the headline result.
2. **Connes's Rigidity Conjecture** — Disproved. Found a counterexample showing groups are not always determined by their von Neumann algebras.
3. **Sphere Packing** — Enhanced asymptotic bounds reaching the Cohn-Elkies threshold.
4. **Binary and Spherical Codes** — Exponentially stronger bounds at every minimum distance.
5. **Arithmetic Circuit Complexity** — New lower bounds for permanent computation.
6. **Quantum Parallel Repetition** — Exponential repetition for two-player quantum games.
7. **Closest Vector Problem** — Polynomial-factor hardness of approximation.
8. **Ehrhart's Volume Conjecture** — Maximum volume bounds for convex bodies.
9. **Multicolor Ramsey Numbers** — Superexponential lower bounds, resolving Erdős problem #183.
10. **Extremal Graph Theory** — Counterexamples to compactness and degeneracy conjectures.

## Verification methodology

Each solution includes:
- A **Lean 4 formalization** (machine-checkable, line by line, using Lean 4.32.0 + mathlib)
- A **chain-of-thought walkthrough** showing the reasoning
- An **LLM-generated academic PDF** where the model reconstructs how the proof came together from unpublished reasoning traces
- A full academic paper covering all ten results

The Lean proofs are what matter for credibility. Mathematics is hard to fake at the formalization level — Lean is not lenient.

## What this is actually announcing

Astra is OpenAI's planned successor to Sol/Terra/Luna (the current commercial frontier). OpenAI has not decided whether to position it as GPT-6 or as a separate track. Based on this deployment it can:
- Run multi-day autonomous sessions with subagent spawning
- Maintain coherent reasoning across complex, specialized domains without domain-expert supervision
- Self-verify via formal proof generation
- Operate within a project-manager-directed workflow

No commercial release date. No API access. The math post is the announcement.

## Why this matters for Leo

**The pattern is locked in.** Two demonstrations in 72 hours — Claude (crypto weaknesses, July 28) and Astra (math proofs, August 1) — use identical architecture:
- Human sets the goal
- AI does the sustained reasoning, experiments, and iteration
- Human verifies the output

For your fiction pipeline: if the architecture holds for non-sofic group theory and post-quantum cryptography, it holds for maintaining a coherent 20-chapter novel arc. The gaps the Aug 1 PM papers identified (failure to backtrack from dead ends, instruction drift, poor resource awareness) are still the active problems — but the raw capability to do sustained agentic work is now demonstrated by two independent labs.

**The cost floor.** $2,000 for ten solved math problems is not "free" but it's not prohibitive either. The implication for your writing pipeline: sustained multi-chapter runs with full multi-agent pipelines are economically viable at current rates, especially with the 50% usage boost through August 19.

**The Lean 4 scaffolding.** If you ever want to study a real multi-agent root-subagent codebase, `openai/ten-proofs` is Apache-2.0. The agent scaffolding (separate from the Lean proofs) shows how they structured root/subagent hand-offs, retries, and result synthesis.

## Links

- https://openai.com/index/ten-advances-in-mathematics/
- https://github.com/openai/ten-proofs
- https://simonwillison.net/2026/Aug/1/ten-advances-in-mathematics/
- https://the-decoder.com/openai-announces-its-next-major-model-astra-by-dropping-ten-previously-unsolved-math-problems/
- https://gizmodo.com/openai-smuggled-the-announcement-of-astra-its-next-ai-model-into-a-blog-post-about-math-2000793689
