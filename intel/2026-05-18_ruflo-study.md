# Ruflo Study Notes — 2026-05-18

## What it is

**Ruflo** (formerly Claude Flow, by @ruvnet) — multi-agent AI orchestration framework for Claude Code. Adds swarm coordination, self-learning memory, federation, and token optimization on top of vanilla Claude Code.

- npm packages: `ruflo` AND `claude-flow` (legacy name, still published)
- Repo: `github.com/ruvnet/ruflo` (cloned locally to `~/Desktop/ruflo` for study)
- Plugin-based architecture — 32+ plugins in the monorepo
- Featured in the Reel as "ranked #1 on GitHub" — actual star count not verified but it has real traction

## What it actually claims (verified in their CLAUDE.md)

### 3-Tier Model Routing (ADR-026)

| Tier | Handler | Latency | Cost | Use cases |
|---|---|---|---|---|
| 1 | Agent Booster (WASM) | <1ms | $0 | Simple code transforms (var→const, add types) — SKIPS LLM ENTIRELY |
| 2 | Haiku | ~500ms | $0.0002/call | Simple tasks, complexity <30% |
| 3 | Sonnet/Opus | 2-5s | $0.003-0.015/call | Complex reasoning, architecture |

**The 50% token-cut claim from the Reel breaks down as:**
- ReasoningBank retrieval: -32%
- Agent Booster edits: -15%
- Cache (95% hit rate): -10%
- Optimal batch size: -20%

Stacked, these are 30-50% reduction CLAIMED. The numbers are plausible because they're complementary techniques (cache hit + smarter routing + smaller-model-where-safe + batched ops).

### What it's designed for

**CODING swarm orchestration.** The "Anti-Drift Coding Swarm" is their preferred default, hierarchical topology, 6-8 agents max, for collaborative coding work.

Categories of agents:
- Swarm coordination (hierarchical, mesh, adaptive)
- Consensus & distributed (Byzantine, Raft, gossip)
- Performance (benchmarking, optimization)
- GitHub (PR manager, code review swarm, issue tracker)
- SPARC methodology (specification → pseudocode → architecture → refinement)
- Specialized dev (backend, mobile, ML, CI/CD)

## Honest assessment for Leo's stack

### What ruflo is GOOD for

- ✅ **Coding work** — building Jarvis-side infrastructure, writing his own Mac voice-frontend (P3), building skills/plugins, automating dev tasks. The 3-tier routing genuinely cuts costs.
- ✅ **Generic "spawn N agents to research a topic"** workflows — discovery loop style work
- ✅ **Federation** — if he eventually wants Jarvis-on-server + Jarvis-on-laptop to coordinate (P4-and-beyond futures)

### What ruflo is NOT designed for

- ❌ **Long-form writing pipelines.** His existing fixer-chain (R1, R3, R4... R19) + judge panel pattern is a different orchestration shape from ruflo's coding swarm. Forcing his writing pipeline into ruflo's pattern is likely worse than what he has.
- ❌ **Voice DNA / chapter consistency.** Ruflo's ReasoningBank is designed for code patterns, not narrative continuity. His existing voice-dna-words.json + character bibles pattern is more specialized.
- ❌ **Cost-discipline for HIS specific Max-plan/cost-per-chapter math.** Ruflo's $-per-call numbers assume API usage. Leo's on Max plan; the math doesn't translate directly.

## Install paths (from their README)

### Path A — Plugin marketplace only (LITE)

```bash
/plugin marketplace add ruvnet/ruflo
/plugin install ruflo-core@ruflo
/plugin install ruflo-swarm@ruflo
/plugin install ruflo-autopilot@ruflo
```

- Adds slash commands + agent definitions to Claude Code
- ZERO files added to workspace
- MCP server NOT registered — features like `swarm_init`, `memory_store`, `agent_spawn` won't work from Claude
- Safe to try, easy to remove

### Path B — Full install via `npx ruflo init`

```bash
npx ruflo init
```

- Adds `.claude/`, `.claude-flow/`, `CLAUDE.md`, helpers, settings to current directory
- Registers MCP server
- Installs hooks
- Runs a background daemon
- "98 agents, 60+ commands, 30 skills"

**Path B risk for Leo:** he already has CLAUDE.md files in every project (`~/Desktop/ai-writer/<project>/CLAUDE.md`, `~/.claude/CLAUDE.md`, etc.). Running `ruflo init` in any of these could overwrite or conflict. Don't run Path B in any existing project root.

## Recommendation

**Don't full-install ruflo on his system. Don't even Path A right now.**

Instead, **cherry-pick the IDEAS**, three of which are directly transferable:

### Idea 1 — Apply 3-tier routing to his writing pipeline

His current pipeline runs Sonnet (?) for chapter generation + fixers. Applying ruflo's tier-routing logic:
- **Tier 1 (free):** mechanical fixes that don't need an LLM — em-dash strip, regex-based filter-phrase removal, sentence-length checks. Pre-process before any fixer sees the chapter.
- **Tier 2 (Haiku):** Quick scan passes — does this chapter contain X? Does the POV stay locked? These are classification tasks, Haiku is plenty.
- **Tier 3 (Sonnet/Opus):** Actual content generation, nuanced craft calls, judge synthesis.

If his current pipeline runs everything at Sonnet, dropping Tier-1+Tier-2 work to Haiku/regex could legitimately save 30-50% of weekly Max budget for the same output quality.

### Idea 2 — ReasoningBank-style caching

For things that repeat across chapters (character voice patterns, world-bible facts, prose-rule explanations), cache the reasoning instead of re-deriving each time. He probably already does this implicitly via voice-dna files and character bibles, but checking if there are gains in caching MORE (e.g., scene-archetype patterns).

### Idea 3 — Self-learning loop

Ruflo claims agents learn from every task. He could implement a much simpler version: at the end of each chapter, the agent appends 2-3 lines to `state/agent_learnings.md` about what worked and what didn't, and the next chapter's prompt reads that file. Lightweight, no swarm needed.

## Action items (post-finals)

1. **Try Path A in a sandbox.** Make a junk repo, `/plugin install ruflo-core@ruflo` there. Play with 1-2 of their slash commands to see how they actually feel. Don't install in any real project.
2. **Implement Idea 1 (3-tier routing) in his writing pipeline.** Audit current pipeline for: which steps could drop to Haiku without quality loss? Which steps could be pure-code (regex)? This is the highest-EV practical takeaway from ruflo.
3. **Skip Path B (full ruflo install) for now.** Reassess at end of summer if Idea 1 doesn't deliver enough.

## What I didn't validate

- Whether the 30-50% token reduction is real *for non-coding workloads*
- Whether the daemon/background processes are stable
- Cost on Anthropic Max plan specifically (ruflo's docs assume API)
- Whether ruflo's federation feature would conflict with Jarvis's existing architecture

## Cleanup

Local clone at `~/Desktop/ruflo` — ~12MB, no daemon running (we didn't `npx ruflo init`). Safe to leave or `rm -rf` whenever. Recommend leaving for ~1 week of reference, then deleting.

---

*Studied 2026-05-18. Recommendation: cherry-pick ideas, don't full-install.*
