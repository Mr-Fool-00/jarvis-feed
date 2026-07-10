# Briefing: claude-mem v13.10.2 — Persistent Cross-Session Memory (TYPE B — Safety Gate Applied)

**Date:** 2026-07-10 · **Score:** 7/10 (preliminary) · **Type:** B (third-party code)
**Source:** https://github.com/thedotmack/claude-mem
**Build verdict:** DO NOT INSTALL — safety gate. Await Leo's signal. Native build sketch included below.

---

## Safety Gate Status

**BLOCKED — third-party code, cannot auto-install or recommend install.**

Direct WebFetch of the GitHub repo returned 403 from the sandbox proxy. Full README and source code were not deep-dived. Score of 7/10 is PRELIMINARY — actual score after deep-dive may change.

This briefing is for **awareness and native build planning only**. No installation. No `npm install`, `pip install`, or equivalent.

---

## What claude-mem does

Claude-mem is a persistent memory layer for Claude Code using five lifecycle hooks:

- **SessionStart** — loads relevant memories into context at session open
- **UserPromptSubmit** — extracts observations from prompts before they reach Claude
- **PostToolUse** — captures structured observations after each tool call
- **Stop** — compresses session into durable memories before exit
- **SessionEnd** — final cleanup and index update

Storage architecture:
- **SQLite** — primary key-value + metadata store
- **Chroma** — vector DB for semantic retrieval
- **Hybrid retrieval** — semantic similarity + keyword search combined

Memory compression: AI-powered compression collapses verbose session observations into compact, retrievable facts. At v13.10.2 with 296 releases over its history — this is mature, not experimental.

Supports: Claude Code, OpenClaw, Codex, Gemini, Hermes.

---

## Why this matters for Leo's fiction pipeline

The novel pipeline currently rebuilds context from scratch every session. Every time a chapter agent opens, it re-reads the relevant parts of the manuscript plus character notes plus worldbuilding docs. This has two costs:

1. **Token cost** — large context prefixes on every session
2. **Coherence risk** — the model has no memory of what it *reasoned about* in previous sessions, only what it *wrote*. Character motivation drift, worldbuilding inconsistencies, and craft pattern regression are all harder to catch when cross-session reasoning is stateless.

Cross-session memory of:
- Character behavior decisions (why did Yuen act this way in ch. 7?)
- Plot constraints established (what's the hard rule about the bridge scene?)
- Craft feedback (Leo flagged passive voice in ch. 3 — avoid in future)
- Cost-per-chapter history (what did ch. 8 cost in tokens?)

...would directly improve pipeline coherence and Leo's ability to give feedback that sticks.

---

## Native build sketch (recommended alternative)

Rather than installing claude-mem, build a native equivalent:

### Architecture (same as claude-mem but our code)

```
.claude/hooks/
  session_start.py       # load memories → CLAUDE.md injection or --context flag
  post_tool_use.py       # extract observations per tool call
  stop_observer.py       # compress session → memory store
  session_end.py         # index cleanup

state/
  memories.db            # SQLite: id, type, content, tags, created_utc, last_accessed
  memory_index/          # optional: lightweight vector index (no Chroma dependency)
```

### What native avoids

- No third-party package manager dependency
- No Chroma (we can use SQLite FTS5 full-text search as a simpler retrieval layer)
- No external model calls for compression (use CC's own stop hook to compress)
- Full control of what gets stored vs. filtered

### Memory types to track

| Type | Example | Retrieval trigger |
|------|---------|-------------------|
| `character_decision` | "Yuen doesn't hesitate — she acts first" | character name in prompt |
| `plot_constraint` | "Bridge scene must end ambiguously" | scene/location in prompt |
| `craft_feedback` | "Leo: avoid passive voice in action scenes" | any writing task |
| `cost_event` | "Ch. 8: $2.14, 43K tokens, Fable 5" | any chapter task |
| `world_rule` | "Magic costs memory, not energy" | magic/power in prompt |

### Implementation priority

1. Stop hook (compress session → SQLite) — simplest, highest value
2. SessionStart hook (inject relevant memories into context) — second
3. PostToolUse observation extraction — third, optional for V1

This is a two-session build. V1 doesn't need vector retrieval — SQLite FTS5 with tag-based filtering is good enough for a fiction pipeline with ~100 memories.

---

## What Leo needs to decide

1. **Build native vs. wait to deep-dive claude-mem?** — Recommendation: build native. Even if claude-mem is clean after a deep-dive, our native version will be lighter and fully controlled.
2. **What memory types are highest priority for the current novel?** — Character decisions and craft feedback are the most likely to improve cross-session coherence.
3. **OK to proceed with native Stop hook first?** — This is the no-approval-needed piece (native code, no third-party install). Can build immediately once Leo signals interest.

---

*Jarvis briefing · Run 2026-07-10 AM · jarvis-feed-agent*
