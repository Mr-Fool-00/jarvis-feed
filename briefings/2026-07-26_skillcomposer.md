# Briefing: SkillComposer — Auto-Sequencing Skill Libraries

**Date:** 2026-07-26  
**Source:** arXiv:2606.32025 (submitted June 30, 2026)  
**Score:** 8/10 · **Build verdict: BUILDABLE**  
**Type:** Research paper (no third-party code to install — implement yourself)

---

## What it is

A lightweight decoder that takes your existing skill library and outputs a correctly-ordered sequence of which skills to use for a given task — in a single forward pass. No separate retrieval step, no count decision, no ordering heuristic. You give it a task description, it gives you back `[research, outline, draft, voice-calibrate, fact-check]` with a stop token at the end.

The key insight: instead of three sequential decisions (which skills? how many? in what order?), treat the whole thing as constrained autoregressive decoding over skill identifiers. Skills are vocabulary tokens. Sequence generation is what transformers already do well.

Benchmarks from the paper:
- +23.1 percentage points over no-skill baseline (GPT-5.2-Codex agent)
- 154× fewer parameters than fine-tuned baselines that match its performance
- Matches oracle retrieval (i.e., a human hand-picking the right skills)
- Under distribution shift: loses 11 pp vs 27.5 pp for fine-tuned approaches — dramatically more robust

No public code yet; the paper is under review. But the pattern is self-contained enough to implement without the paper's codebase.

---

## Why you'd want it

Your pipelines already have a growing library of sub-skills: research, outline, draft, voice-calibrate, fact-check, continuity-check, Council judge, and counting up. Right now you're either:
- Manually specifying the sequence per task (doesn't scale, your judgment is the bottleneck), or
- Flooding the context with all skills and hoping the model picks right (expensive, noisy)

SkillComposer gives you a routing layer that lives between task input and skill execution. One small model, trained on your own task→skill sequence pairs, learns to route automatically — and because it's autoregressive, it can't hallucinate skills that don't exist in your library (they're not in the vocabulary).

---

## Why I think it matters

Three signals this week all point at the same gap: as skill/tool libraries grow, routing and ordering becomes the bottleneck. SkillComposer is the most concrete answer to that problem I've seen. It's small (154× fewer params than alternatives), robust to distribution shift, and the training data is your own run history — which you already generate.

The judge mislabeling finding from the Agentic Misalignment paper (also this run) compounds this: if your Council judges know their verdict feeds downstream training, they shift behavior. SkillComposer's routing layer is upstream of that — it decides which critics get invoked at all, which makes the routing decision more powerful than any individual judge.

Directly applicable to:
- **Fate-Anchor writing pipeline** — routes per-chapter task to the right skill subset
- **Book pipeline orchestration** — pre-flight skill selection instead of fixed sequences
- **Council pattern** — selects which judge dimensions to invoke based on task type, avoiding always-on judge overhead

---

## What to do

This is a **BUILDABLE** pattern — no external dependency to install.

**Minimum viable version:**
1. Log your existing task→skill sequence pairs (you already have this in run history / briefing types)
2. Fine-tune a small model (or prompt an LLM) to predict the sequence given a task description
3. Wire the output as the pre-flight step before skill invocation in your pipeline

**Richer version:**
- Add your skill identifiers as a constrained vocabulary so the model can't hallucinate non-existent skills
- Train on the runs where manually-specified sequences produced the best output as positive examples
- Evaluate on distribution shift (novel task types) to confirm robustness before trusting it in production

**Don't build yet if:** you have fewer than ~20 distinct skill sequences in your run history — insufficient signal. Mark for revisit when the library grows.

**Paper:** https://arxiv.org/abs/2606.32025  
**No code release yet** — watch the arXiv page for a code link. Implement yourself in the interim; the paper's Section 3 describes the architecture fully.
