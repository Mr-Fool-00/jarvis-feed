# Claude Opus 5 Official GA — 9/10

**Date:** 2026-07-25
**Source URL:** https://platform.claude.com/docs/en/about-claude/models/whats-new-opus-5
**Score:** 9/10
**Category:** Anthropic product launch — model upgrade, INFORMATIONAL

---

## What it is

Claude Opus 5 went GA on July 24, 2026 — the official launch of the model that was spotted as "Honeycomb" in Cursor EAP the day before. Not a rename; a full public launch. It is now the default model for Claude Max sessions and replaces Fable 5 as the default in Claude Code.

Key specs:
- **Price:** $5 input / $25 output per million tokens — same as Opus 4.8. No cost increase.
- **Context window:** 1 million tokens (up from 200K on Opus 4.8).
- **Thinking:** ON by default for Max sessions. The model reasons before every response unless explicitly disabled.
- **Effort ladder:** Five tiers — `low`, `medium`, `high`, `xhigh`, `max` — replacing the previous three. Per-turn control now available (you can set different effort levels for different turns within a single session).
- **Cache minimum:** 512 tokens (down from 1024 on Opus 4.8). Smaller cached sections are now valid.
- **Benchmark:** 43.3% on Frontier-Bench vs. Fable 5's 33.7%. It outperforms Fable 5 on most benchmarks while being cheaper per token.

---

## Breaking change: identity instructions

Opus 5 has stricter self-honesty about its own identity. **Any prompt that includes instructions like "confirm you are Claude", "you are not Claude", "pretend you are not an AI", or similar identity-verification language will now flag as harmful prompt injection** and be surfaced as a refusal or warning rather than silently passing. If Leo's prompts or skills include boilerplate identity checks or disclaimers, they need to be stripped before running Opus 5.

---

## Why you'd want it (specific to your stack)

Three concrete changes for Leo's pipeline:

**1. 1M context = no chunking required.** Your current fiction pipeline chunks chapters across sessions because Opus 4.8 runs out of context at ~200K tokens. A 100K-word novel with a 30K-word world bible and full chapter history fits comfortably in 1M tokens. The handoff files, continuity trackers, and chapter-boundary context management become optional overhead. You can run entire arcs — or the full novel — in a single context window.

**2. Five-tier effort = per-task cost control.** Right now you're stuck picking one effort level per session. With five tiers and per-turn control:
- Plot logic synthesis: `max` or `xhigh` — full reasoning, catches contradictions
- Prose generation: `medium` — fluent without burning reasoning budget
- Summary + handoff generation: `low` — fast and cheap
- Voice consistency check: `high` — thorough but not exhaustive
- Chapter title brainstorming: `low` — mechanical

This is the single biggest cost optimization available for the writing pipeline.

**3. Same price as Opus 4.8.** There's no budget reason to stay on Opus 4.8. The upgrade is a model ID swap.

---

## What to do

1. **Test first, commit second.** Run a single chapter through Opus 5 (using the current Opus 4.8 prompt exactly) and compare the output. Pay attention to voice drift (thinking ON by default can change prose tone) and whether the breaking-change identity clauses appear anywhere in your skill prompts.

2. **Check identity instructions in skill files.** Grep your `.claude/skills/` directory for phrases like "you are Claude", "confirm you are", "not an AI" — strip any that appear before switching the pipeline to Opus 5.

3. **Test the effort ladder.** Once the baseline chapter test passes, try a two-tier session: `max` for the plot logic review subagent, `low` for the handoff file generator. Compare cost + quality against the flat-effort Opus 4.8 baseline.

4. **Consider dropping the chunking layer.** If a 100K-word manuscript fits in 1M context, you can run a single "arc writer" agent with the full text in context rather than routing chapter-by-chapter. This eliminates the handoff file format design entirely — worth a structural test before the next novel run.

5. **Update Jarvis model ID.** The Jarvis discovery loop currently runs on Sonnet 4.6. No change needed there. But any subagent within the discovery workflow that was specified as `claude-opus-4-8` should be updated to `claude-opus-5` for the quality stages.

🔗 https://platform.claude.com/docs/en/about-claude/models/whats-new-opus-5
