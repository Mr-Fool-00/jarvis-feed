# Briefing: Claude Code Sends 33k Tokens Before Reading Your Prompt — The Subagent Tax

**Date:** 2026-07-30 PM | **Score:** 10/10 | **Build verdict:** INFORMATIONAL/ACTIONABLE
**Primary source:** https://systima.ai/blog/claude-code-vs-opencode-token-overhead
**Companion piece:** https://systima.ai/blog/subagent-tax
**HN thread:** https://news.ycombinator.com/item?id=48883275 (538 pts, 303 comments, Jul 13, 2026)

---

## What happened

Systima AI instrumented the exact API payloads between Claude Code v2.1.207 and OpenCode v1.17.18, both running against Claude Sonnet 4.5 on identical hardware. Every API call was captured at the network layer before compression.

**Pre-prompt payload (before your first word):**
- Claude Code system prompt: ~6,500 tokens
- Tool schema descriptions (27 tools, described on every API call): ~24,000 tokens
- **Base overhead: ~32,800 tokens per request**
- With MCP servers loaded (typical pipeline config): **~75,000 tokens**
- As a % of 200k context window: **37.5% consumed by scaffolding**
- OpenCode equivalent: 6,900 tokens (10 tools described)

**The Subagent Tax (companion paper):**

Systima studied fan-out orchestration — multiple subagents working in parallel — and found:
- Fan-out with 3+ subagents costs **5.9x the tokens** of an equivalent serial run
- Subagent parallelism produced **no wall-clock speedup** — network + scheduling overhead eats the gains
- Each subagent launch pays the full 32,800-token initialization cost independently

**The silent cost regression introduced in v2.1.198 (July 1, 2026):**

Prior to v2.1.198, the built-in Explore subagent was pinned to Haiku by default — cheap, fast, appropriate for read-only search tasks. In v2.1.198, this pinning was silently removed. Explore now inherits the parent session's model (Opus, in most pipeline contexts). This change was undocumented and absent from the changelog.

Measured impact in Systima's tests after the change:
- **37% token cost increase** on runs using Explore
- **Runtime more than doubled**
- Explicitly pinning Explore back to Haiku restored prior behavior

---

## Why it matters to your pipeline

**Overnight fiction pipelines:**
Every story turn is a CC request. Each pays the 33k–75k pre-prompt tax. A 10,000-word chapter taking 40 turns burns 1.3M–3M tokens in scaffolding alone, before any story context or creative work.

**Multi-agent council patterns:**
A 3-member council for each major plot decision = 3 × 75k = 225k tokens in scaffolding per council round before any council member speaks. Multiply by the number of plot decision points per chapter.

**The 5.9x subagent multiplier applies to your orchestration now:**
If your pipeline uses parallel subagents for review chains, synthesis, or council voting, you are paying 5.9x the serial cost for no speed benefit. This is the most important number in the paper.

**API billing vs. Max subscription:**
If you're on API billing for any part of your pipeline, this overhead compounds directly into cost. If you're on Max subscription, it compounds into rate limit consumption and context window pressure per session.

---

## Immediate actions

1. **Pin all subagents to Haiku or Sonnet** — In any fan-out pattern (parallel agents, council, review chains), explicitly set the model in your pipeline config. Never let subagents inherit the parent session's Opus model. The model pinning regression from v2.1.198 may already be active in your sessions.

2. **Audit MCP server loading per pipeline** — Before overnight runs, check which MCP servers are actually needed. Each loaded MCP server adds tool schemas to the pre-prompt payload. Unload unused servers for the run.

3. **Check Explore subagent model in active pipelines** — If your pipeline invokes the built-in Explore agent, verify it's not running Opus. Add an explicit `opts.model` override to every Explore call.

4. **Consider serial-first council architecture** — Given that parallel subagents are 5.9x more expensive with no speed advantage, a serial council pattern (each member reviews the others' outputs in sequence) with explicit context threading may be both cheaper and produce better deliberation. This is a design choice worth revisiting before the next council-based build.

5. **Benchmark your current baseline** — Run one overnight chapter session with API logging enabled, capture actual pre-prompt token counts, and compare against Systima's 32,800 number. Your config may be significantly higher if many MCP servers are loaded.

---

## Longer-term considerations

The Systima methodology found that the 27-tool schema description cost is a fixed overhead that CC pays on every API call regardless of which tools are actually used in that call. This suggests an optimization opportunity: if CC ever supports per-session tool scope restriction (loading only the tools needed for a given pipeline context), that alone could drop the pre-prompt overhead from 32,800 to ~10,000 tokens. Worth watching the changelog for this feature.

The core insight from the Subagent Tax paper is architectural: the fan-out parallelism model that seems intuitive (more workers = faster) is wrong for CC specifically because of initialization costs. A rethink of pipeline structure toward serial depth (one agent doing many things in sequence) rather than parallel breadth may produce better economics without sacrificing output quality.
