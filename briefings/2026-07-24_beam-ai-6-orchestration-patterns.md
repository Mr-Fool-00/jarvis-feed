# Briefing: Beam.ai — 6 Multi-Agent Orchestration Patterns for Production

**Date:** 2026-07-24 (AM run)
**Score:** 8/10
**Category:** Multi-agent architecture · orchestration patterns · production guide
**Build verdict:** BUILDABLE — design spec for `/book-pipeline` v2; council pattern cost math validates Opus 4.8 economics; read before next overnight run

---

## What it is

Beam.ai published a production-focused catalog of six multi-agent orchestration patterns with cost tradeoffs for each. This isn't academic — it's a practitioner guide with concrete numbers.

The six patterns:

1. **Sequential chain** — agents run one after another; cheapest, lowest reliability
2. **Parallel fan-out** — same input → N agents simultaneously; good for independent tasks
3. **Council** — multiple models (e.g. Sonnet 4.6 + GPT-5.4) run the same task in parallel, a judge model arbitrates the winner; best output quality
4. **Hierarchical** — parent orchestrator decomposes → child agents → parent aggregates; good for complex multi-step tasks
5. **Event-driven** — agents wake on events/triggers rather than a central orchestrator polling; best for long-running workflows
6. **Critic/Judge pipeline** — a "critic" agent evaluates output and passes feedback; a "judge" agent makes binary go/no-go decisions

---

## The three patterns that matter most for Leo's pipeline

### Council pattern (Pattern 3)

Run Claude Sonnet 4.6 and GPT-5.4 in parallel on the same task. A separate judge model reads both outputs and picks the winner (or synthesizes a better answer from both).

**Cost math from the article:** Council adds ~2.5× cost versus a single writer, but measurably improves output reliability. At Opus 4.8 fast-tier pricing ($10 input / $50 output per M tokens), running a 2-model council with a Haiku judge adds roughly:

- Single Sonnet draft: baseline
- Council (Sonnet + GPT-5.4 + Haiku judge): ~2.5× baseline
- Verdict: cost is real but manageable for chapter work on Max plan; not worth it for outlines or scene summaries

Microsoft Copilot Council uses this pattern in production.

### Critic vs. Judge role separation (Pattern 6)

This is the most commonly confused distinction in pipeline design:

| Role | Output | Gate authority |
|------|--------|---------------|
| **Critic** | Suggestions, improvement notes, feedback | None — advisory only |
| **Judge** | Binary GO / NO-GO decision | Yes — blocks pipeline progress |

Mixing these roles in a single agent is the source of most quality-gate failures: the agent hedges its critique ("this could be better") when it should be making a firm decision ("REJECT — voice inconsistency in paragraphs 3-7"). The fix is to separate them into two distinct agents with explicit role prompts.

**In Leo's pipeline:** the current quality gate agent is acting as both critic and judge. Splitting this into a Critic (emits structured notes) + Judge (emits APPROVED / REJECTED + reason) is the single highest-leverage architectural change for quality-gate reliability.

### Fixer pipeline (Pattern 6 extension)

Judge flags REJECTED → Fixer agent reads the judge's reason + original draft → applies targeted patches → output re-enters the pipeline at the critique stage.

The article documents the termination signal pattern (same as BrightCoding briefing):
- Fixer emits `APPROVED` when it believes the draft now meets gate criteria
- Pipeline exits after 3 consecutive failures (escalation, not infinite loop)

---

## Why this matters now

**Four independent items this run all converge on the same architecture:** the Beam.ai guide, OrchRM (arXiv:2606.13598), aaddrick/claude-pipeline (GitHub), and event-driven multi-agent arXiv paper. Practitioners and researchers are arriving at council + judge + fixer role separation independently. This is the strongest single signal that the pattern is stable enough to build on.

**The 3W+3E+4R book-pipeline design from prior briefings maps directly to this:**
- 3 Writers = council (Fable 5 / Sonnet 4.6 / cross-check)
- 3 Evaluators = separate critics per dimension (voice / continuity / dialogue)
- 4 Revisers = fixer pipeline per evaluation dimension

**Opus 4.8 economics validate the council approach:** at $10 input / $50 output (67% cheaper than Opus 4.7), running a 2-model council becomes affordable within Max plan budget for chapter-level work. The cost math the article cites was written before the Opus 4.8 price drop, making the real cost even lower.

---

## Build sequence for `/book-pipeline` v2

This briefing is the design spec. When Leo reacts 🚀 to the digest item, build in this order:

1. **Critic/Judge split** (30 min) — fork the existing quality-gate skill into two: `critic.skill.md` (returns structured notes) and `judge.skill.md` (returns APPROVED/REJECTED + reason). Test on one chapter.

2. **Fixer pipeline** (45 min) — write `fixer.skill.md` that reads judge reason + draft, applies targeted patches. Wire the loop with 3-attempt limit.

3. **Council pattern** (60 min) — add a second writer (Sonnet 4.6 or via an alternate prompt) and a judge that compares both outputs. Start with the chapter opening only to validate cost/quality before enabling full-chapter council.

4. **Wire into `/book-pipeline`** (30 min) — replace single-writer step with council invocation; replace quality-gate with Critic → Judge → Fixer loop.

**Cost estimate:** Full council run on a 3,000-token chapter draft using Sonnet 4.6 + Fable 5 + Haiku judge ≈ $0.35-0.45 per chapter. At 30 chapters, ~$12-14 total. Well within Max plan.

---

## Safety gate

This is an **architecture reference**, not installable code. No safety gate needed. The patterns are extracted from the Beam.ai article and adapted for Leo's pipeline; no third-party code is being copied or installed.

Source: https://beam.ai/agentic-insights/multi-agent-orchestration-patterns-production
