# Briefing: Claude Code Cost Management Before Sept 1

**Items:** CC Token Optimization: 19 Changes (Build to Launch Substack) + CC Usage Limits Playbook 2026 (Developers Digest)  
**Deadline:** August 31, 2026 (Sonnet 5 pricing cliff — see also `briefings/2026-08-03_sonnet5-pricing-cliff.md`)  
**Score:** 7/10 each  
**Build verdict:** ACTIONABLE — implement highest-impact changes before end of month

---

## Two Distinct Cost Layers

Claude Code exposes two separate cost meters that most users conflate:

1. **API token cost** — what you pay Anthropic per token. Affected by model choice, prompt length, output verbosity. The Sept 1 Sonnet 5 increase (+50% sticker) lives here.
2. **CC usage allowance** — rate limits built into CC subscriptions (Max/Pro/Team). Opus 5 costs 5× a Sonnet 5 call from this limit's perspective, regardless of what you pay per token. Haiku 4.5 costs ~0.2×.

The two playbooks this window address these separately. Both matter before Sept 1.

---

## Layer 1: Reducing Token Cost (Build to Launch: 19 Changes)

The article identifies 19 changes; the highest-impact subset:

### 1. Prune CLAUDE.md aggressively (est. 5–10% per-session saving)

Every byte in CLAUDE.md appears in every context window, every session. The article demonstrates:
- 4KB CLAUDE.md → 1.2KB (removing verbose explanations, keeping only directive lines) = **~8% reduction in per-session input tokens** on long sessions
- Rule of thumb: if a line in CLAUDE.md explains *why* a rule exists, cut it. The agent doesn't need the rationale at runtime — it needs the constraint.

**Jarvis-specific action:** Audit `CLAUDE.md` and `AGENT_RUNBOOK.md` for prose paragraphs that could become bullet directives. This is one of the cheapest wins.

### 2. Set `/compact` threshold earlier (est. 3–8% saving on long sessions)

Default triggers at ~75% context fill. Setting to 50% means earlier compression, reducing expensive tail-of-context reads. Tradeoff: slightly more aggressive summarization, which may lose fine-grained detail in very long sessions.

CC setting: `compactContextThreshold: 0.5` in `~/.claude/settings.json` (or project-level `.claude/settings.json`).

### 3. Route mechanical subagents to Haiku 4.5 (est. 40–80% cost reduction on those steps)

Haiku 4.5 at $0.80/$4 per MTok is 3.75× cheaper on input vs. Sonnet 5 standard. For steps that don't require deep reasoning:
- File scanning / glob matching
- JSON extraction / structured formatting
- Classification / tagging
- Deduplication checks (e.g., seen.json lookups)

The article reports no quality regression on these task types when routing to Haiku 4.5.

**Jarvis-specific action:** The scoring step (0–10 item scoring) runs on Sonnet 5. The deduplication check (is this ID in seen.json?) runs on Sonnet 5. The latter is a Haiku 4.5 candidate — it's a pure lookup task.

### 4. Set explicit output token budget per subagent step

Leaving max output tokens uncapped means a verbose subagent can run up a large output bill. Setting `--max-output-tokens` (or the API equivalent) per step to the actual expected output size prevents runaway verbosity.

For Jarvis: the scoring step returns a JSON object with ~5 fields. Capping at 500 output tokens per item (vs. uncapped ~4K default) is the right call.

### 5. Prompt caching: batch related work into single sessions

CC caches the system prompt + CLAUDE.md, but only for sessions long enough to hit the cache threshold. Short sessions (< ~5 exchanges) never benefit from caching.

**Implication:** Starting a fresh CC session for every 5-minute task wastes the cache investment. Batching related queries — e.g., running the fetch, score, and write steps in one continuous CC session rather than three restarts — means the expensive CLAUDE.md/system-prompt tokens are only charged once.

### 6. `--max-context-tokens` flag (experimental as of v2.1.219)

Hard-limits the context window available to a CC session or subagent. Useful for parallel agents that don't need full shared state — e.g., a fetch subagent that only needs the SOURCES.yaml and a single URL doesn't need the full session context. The article reports this flag is experimental and may not persist in all CC versions.

---

## Layer 2: Managing CC Usage Limits (Developers Digest Playbook)

Separate from API costs, CC subscriptions have a per-session usage allowance. **The model-tier multiplier:**

| Model | CC Usage Cost (relative) |
|-------|--------------------------|
| Haiku 4.5 | ~0.2× |
| Sonnet 5 | 1× |
| Opus 5 | **5×** |
| Fable 5 | ~10× |

A CC session that defaults to Opus 5 (now the CC default since v2.1.219) consumes its usage allowance 5× faster than Sonnet 5. This is independent of what you pay per token — it's a rate limit.

### Key techniques from the playbook:

**Switch model mid-session when approaching limits:**
`/model sonnet-5` switches the active model without losing session context. If you're deep in a long Opus 5 session and approaching usage limits, switching to Sonnet 5 for the mechanical tail steps (writes, commits, summary formatting) preserves the session while slowing allowance burn.

**Use `--resume` to avoid burning preamble tokens repeatedly:**
`claude --resume <session-id>` picks up a previous session. The preamble (CLAUDE.md, system prompt) is served from cache; you don't re-pay to re-establish context. For Jarvis's twice-daily runs, each run is a fresh session — but for interactive work, resume is significant.

**Organization-level model locks (Team tier):**
Admins can set per-user model defaults via the admin console, preventing individual contributors from defaulting to Opus 5 on bulk tasks. Not applicable to single-user setups but noted for completeness.

---

## Jarvis-Specific Recommendations

Given the Sept 1 deadline and current Jarvis architecture:

| Step | Current Model | Recommended | Reason |
|------|--------------|-------------|--------|
| Fetch/search synthesis | Sonnet 5 | Sonnet 5 (keep) | Throughput-sensitive, quality matters |
| Item scoring (0–10) | Sonnet 5 | Sonnet 5 (keep) | Judgment call, keep quality |
| Deduplication lookup | Sonnet 5 | Haiku 4.5 (move) | Pure lookup, no judgment needed |
| Digest writing | Sonnet 5 | Opus 5 (consider) | Highest quality sensitivity, one call/run, cost delta is small |
| Briefing writing | Sonnet 5 | Opus 5 (consider) | Same reasoning as digest |
| seen.json update | Sonnet 5 | Haiku 4.5 (move) | Structured JSON edit, no judgment |
| Heartbeat/commit | Sonnet 5 | Haiku 4.5 (move) | Mechanical string operations |

**CLAUDE.md/AGENT_RUNBOOK.md audit** is the single highest-leverage change: it reduces every session's cost with zero tradeoff on quality.

---

## Companion Reading

This briefing pairs with: `briefings/2026-08-03_sonnet5-pricing-cliff.md` — covers the API-tier pricing math (sticker price, tokenizer inflation, model comparison post-Sept 1). Read that first for the pricing context; this briefing covers the mechanics of reducing consumption.

---

## Action Items

1. **This week:** Audit `CLAUDE.md` — strip explanatory prose, keep only directives. Target: under 1.5KB.
2. **Before Aug 15:** Set `compactContextThreshold: 0.5` in CC settings.
3. **Before Aug 25:** Identify Jarvis steps that can route to Haiku 4.5 (dedup lookup, JSON updates, heartbeat writes).
4. **Before Aug 31:** Decide whether digest/briefing writing should route to Opus 5 post-Sept 1 (quality vs. 1.67× cost premium vs. Sonnet 5 standard).
5. **Optional:** Add per-run token count to Jarvis state so Sept 1 impact is measurable against baseline.

---

*Briefing generated by Jarvis · 2026-08-04 AM run · Sources: Build to Launch Substack, Developers Digest (both via WebSearch) · Companion: briefings/2026-08-03_sonnet5-pricing-cliff.md*
