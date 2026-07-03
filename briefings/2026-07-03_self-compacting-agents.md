# Briefing: Self-Compacting Language Model Agents — Model-Driven Context Management

**Date:** 2026-07-03
**Score:** 7/10
**Paper:** arXiv:2606.23525 (SelfCompact)
**Verdict:** BUILD-WORTHY — directly addresses the context window failure mode in long overnight fiction runs. Implementable without fine-tuning.

---

## The problem

Long agent runs accumulate junk. By chapter 8 of a 12-chapter overnight fiction run, the context window is carrying:
- Initial character sheets (already processed)
- Earlier chapter drafts (already committed)
- Intermediate revision notes (superseded)
- Tool call results from 3 hours ago (stale)
- Partial outlines that were replaced

This stale material anchors the model. It doesn't just waste context space — it actively degrades generation quality because the model is still attending to decisions that were already resolved.

Current practice: fixed-interval compaction every N tokens, regardless of what's happening. That means the model might get compacted mid-scene (destroying reasoning state it needs to backtrack) or not compacted after a chapter finishes (carrying forward material it's done with).

---

## What SelfCompact does

It turns compaction into a **tool the model can call**. Instead of external triggers at fixed intervals, the model itself decides when to compact.

Two components:

**1. A compaction tool**
When invoked, it makes an extra LLM call to summarize the accumulated context, then replaces the full accumulated trace with the summary. KV-cache reuse makes this extra call cheap — it doesn't meaningfully increase latency.

**2. A natural-language rubric**
A short paragraph (not training, not code) that tells the model:
- **When to compact:** a sub-task has resolved; the trajectory is converging on a conclusion; you've finished a section and are moving to the next
- **When NOT to compact:** you're mid-derivation; you're stuck and might need to backtrack; you're in the middle of a scene

**The rubric is what makes it work.** Giving the model the tool alone produces inconsistent compaction behavior. Adding the rubric closes the gap.

---

## Results

- **Competition math benchmark:** +18.1 accuracy points over no-compaction baseline
- **Agentic search (BrowseComp-Plus):** +5–9 accuracy points with 30–67% lower per-question token cost
  - GLM-4.7-Flash: 67% cost reduction
  - MiniMax-M2.5: 63% cost reduction
  - Mimo-V2-Flash: 33% cost reduction

No fine-tuning required. The same model running the task also runs the compaction.

---

## How to apply this to Leo's fiction pipeline

The fiction pipeline has natural stage boundaries that map directly to the rubric:

**Compact after:**
- A scene is complete (the scene resolved; move forward without carrying intermediate drafts)
- A chapter is complete (chapter structure, draft, and revision pass are done; carry forward only the final text and the world-state delta)
- A revision pass is complete (before starting the next revision cycle)

**Don't compact:**
- Mid-scene (the model is in active generation; it may need to reread earlier parts of the scene to maintain internal consistency)
- Mid-revision (the model is comparing original and revision; compacting destroys the original for comparison)
- When stuck (if the model is producing similar output repeatedly, it may need its accumulated context to diagnose why)

**Implementation path:**
1. Write a CLAUDE.md section (or fiction pipeline SYSTEM prompt addition) with the compaction rubric above
2. Expose a compaction tool that summarizes and truncates the context — in CC, this can be a simple slash command or hook
3. Let the model call it after chapter completions and scene transitions

This is a half-day implementation. The 30–67% token cost reduction means overnight runs that currently hit rate limits before completing might complete cleanly.

---

## The honest note on effort

SelfCompact as described in the paper is implemented in code (the compaction tool is a real tool call). In a CC context, the equivalent is either a hook that runs at stage boundaries or a skill Leo can invoke manually. The rubric-only piece (telling the model when to compact in its system prompt) is free to implement right now. The tool-call piece requires a CC hook or skill implementation.

Start with the rubric. Add the tool when there's a concrete overnight run that's hitting context limits.

🔗 https://arxiv.org/abs/2606.23525
🔗 https://huggingface.co/papers/2606.23525
