# Briefing: Simon Willison llm-coding-agent — Model Routing Heuristic

**Date:** 2026-07-07  
**Score:** 7/10  
**Verdict:** BUILDABLE  
**ID:** `rss:simonwillison:llm-coding-agent-model-routing-july2`  
**Source:** https://simonwillison.net/2026/Jul/2/llm-coding-agent/  
**build_worthy:** TRUE

---

## What it is

On July 2, Simon Willison published `llm-coding-agent` 0.1a0, a standalone coding agent library extracted from his LLM framework. The library itself is straightforward. More important is the **model routing principle** he stated July 3:

> "For all coding tasks use your judgement to decide an appropriate lower power model and run that in a subagent — reserve top-tier model for judgment/review/synthesis in the main loop."

---

## The routing heuristic, made concrete

| Task type | Model tier | Rationale |
|-----------|-----------|-----------|
| Orchestration, planning, judgment calls | Fable 5 / Opus 4.8 | Needs deep reasoning; rare calls; cost justified |
| Code generation, execution, writing | Sonnet | Quality output, 5× cheaper than Fable |
| Parsing, extraction, structured output | Haiku | Mechanical tasks; 20× cheaper than Fable |
| Review, synthesis across subagent outputs | Fable 5 / Opus | Main loop; this is where top-tier pays off |

---

## Why this matters now — the July 8 pricing cliff

From the AM digest: Fable 5 moves to credits-only July 8 at **$10/M input, $50/M output** — double Opus 4.8's cost.

If the Jarvis cron runs Fable on every agent call in a workflow, and a typical workflow spawns 10–20 agents, a single run costs:
- 10 agents × 10K output tokens each = 100K output tokens
- At $50/M: **$5 per cron run**
- Jarvis runs twice daily: **~$300/month just in Fable output costs**

Same run with Sonnet doing generation (Fable only for judgment):
- 1 Fable agent × 5K output = 5K output tokens at $50/M = **$0.25**
- 9 Sonnet agents × 10K output = 90K output tokens at $15/M = **$1.35**
- Total: **~$1.60/run** → ~$96/month

That's a 3× cost reduction with no quality loss on generation tasks.

---

## Concrete implementation for Jarvis workflows

In any workflow script:

```javascript
// High-judgment orchestration calls → Fable
const rankedItems = await agent(rankingPrompt, {
  model: 'claude-fable-5',
  label: 'rank-and-filter'
});

// Generation/writing → Sonnet  
const briefings = await parallel(items7plus.map(item => () =>
  agent(briefingPrompt(item), {
    model: 'claude-sonnet-5',  // or claude-sonnet-4-6
    label: `briefing:${item.id}`
  })
));

// Parsing/extraction → Haiku
const parsed = await agent(parseSourcesPrompt, {
  model: 'claude-haiku-4-5-20251001',
  label: 'parse-rss-feeds'
});
```

---

## Caveats

- This only matters if Fable 5 credits-only stays in effect. If Anthropic restores standard Max access "as capacity allows," the cost math changes.
- Sonnet 5 is the current recommended generation model (latest); don't use Sonnet 4.6 for new work.
- The `llm-coding-agent` library itself is Python, not directly applicable to CC workflow scripts (JS). The routing principle is portable, the library is not.

---

## Recommended action

Apply the tiered model routing to all Jarvis workflow scripts starting from the next run after July 8:
1. Main loop + judgment calls → `claude-fable-5` (or `claude-opus-4-8` as fallback)
2. Writing/generation subagents → `claude-sonnet-5`
3. Parsing/extraction → `claude-haiku-4-5-20251001`

**Leo approval required before modifying workflow scripts** (per Step 4.5 safety gate — this is a behavior change to existing automation, not third-party code installation, but the principle applies: confirm before touching production cron logic).
