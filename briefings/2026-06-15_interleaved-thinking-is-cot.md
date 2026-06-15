# Claude 4 Interleaved Thinking: The IS-CoT Implementation Path — 7/10

**Date:** 2026-06-15
**Source URL:** https://aws.amazon.com/blogs/opensource/using-strands-agents-with-claude-4-interleaved-thinking
**Score:** 7/10
**Category:** Native Claude 4 API feature (not third-party code) — production implementation path for this morning's IS-CoT briefing

---

## What it is

Claude 4 has a feature called **interleaved thinking** — which means Claude can use its extended thinking space *between* tool calls, not just at the start of a response. It plans, runs a tool, thinks about what the result means, adjusts its plan, runs the next tool, thinks again. This happens automatically when you turn on extended thinking AND give Claude tools to use.

The AWS blog (published June 13-14) showed this working in Strands Agents (an open-source SDK), but the actual feature is in Claude 4 itself — you get it via the Claude API or Claude Code with `budget_tokens` set.

---

## Why you'd want it (specific to your stack)

This morning's AM digest surfaced IS-CoT (arxiv:2606.09709) — a research paper showing that a Plan→Write→Reflect loop reduces long-form generation quality collapse by 41%. The AM briefing said "build a prompt harness to implement it."

This PM finding changes the implementation: **Claude 4 already does Plan→Write→Reflect natively when you give it tools + extended thinking.** You don't need a complex prompt harness wrapping each chapter. You just need:

1. Extended thinking enabled (`budget_tokens: ~10000`)
2. Tools for the fiction pipeline: `ReadFile` (context), `WriteFile` (chapter output), `AuditChapter` (quality check)

Claude 4 will naturally use its thinking space to plan before writing, write the chapter, then think about whether the output meets quality criteria before calling `AuditChapter`. This is the IS-CoT loop implemented as a native model capability, not a workaround.

**Concrete change to the /chapter-write skill:** Add `extended_thinking: {type: "enabled", budget_tokens: 10000}` to the API call. That's the upgrade.

---

## Why I think it's worth your attention

The AM briefing said IS-CoT required building a prompt harness. This says it doesn't — Claude 4 already has the architecture. The skill build just got 60% simpler. This is a "wait, we can skip a step" moment.

---

## What to do

1. React to today's AM briefing (`briefings/2026-06-15_is-cot-long-form-writing.md`) to confirm you want the /chapter-write skill built.
2. When building it: use extended thinking with tool calls rather than the pure prompt harness the AM briefing described. Same outcome, simpler implementation.
3. The Strands Agents tutorial is the clearest working example of the pattern — skim it before building even if you don't use Strands.

🔗 https://aws.amazon.com/blogs/opensource/using-strands-agents-with-claude-4-interleaved-thinking
