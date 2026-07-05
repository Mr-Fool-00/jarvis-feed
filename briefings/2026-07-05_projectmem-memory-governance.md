# Briefing: PROJECTMEM — Memory-as-Governance for AI Coding Agents

**Item:** PROJECTMEM: A Local-First, Event-Sourced Memory and Judgment Layer for AI Coding Agents  
**Source:** arXiv:2606.12329 (June 2026)  
**URL:** https://arxiv.org/abs/2606.12329  
**Score:** 8/10 · **Digest:** 2026-07-05_PM · **Run:** PM  
**Safety gate:** Third-party code — do not install without Leo's review. Pattern is buildable natively.

---

## The core pattern

PROJECTMEM is built around one architectural insight: agent memory should be *typed* and *governed*, not just stored and retrieved.

**What it does:**
1. Every coding session generates an append-only event log with typed events: `issue`, `attempt`, `fix`, `decision`, `note`
2. The log is deterministically projected into compact AI-readable summaries
3. These summaries are served via MCP to the agent at session start
4. A **pre-action gate** checks every proposed action against prior session history

**The pre-action gate is the novel piece.** Before the agent writes to a file, it gets a context injection: "You tried this approach in session 3 and it caused a regression in the test suite." Before it proposes a fix, it sees: "This file is marked as fragile — two prior attempts at this change were reverted." The gate doesn't block — it warns. The agent can proceed, but it knows it's treading known-difficult ground.

**Verified working with:** Claude Desktop, Cursor, Antigravity, Codex. 14 MCP tools, 19 CLI commands. 3-dependency Python package. 100% local, no cloud, MIT license.

**Research validation:** 2-month dogfooding study across 10 real projects, 207 events logged. Published findings show the pre-action gate intercepted 23% of proposed actions with relevant prior-session context.

---

## Direct remapping to fiction pipeline

The pattern translates almost one-to-one from coding to fiction:

| Coding event | Fiction equivalent |
|---|---|
| `issue` | Unresolved plot hole, character contradiction, continuity gap |
| `attempt` | Revision approach tried ("made confrontation a dialogue — felt false") |
| `fix` | What resolved the issue ("moved confrontation to Act 3 payoff") |
| `decision` | Authorial choice locked in ("Chapter 3 stays third-person limited") |
| `note` | Observation without action ("reader may find the time jump confusing") |

**Pre-action gate for fiction:** Before generating a new scene, the gate checks: "Have you tried writing this scene before? What approach failed? What decisions constrain this scene?" If Chapter 6's failed drafts are logged, the agent doesn't walk into the same trap.

This is the **cognitive debt** pattern (Willison's "Understand to Participate" post, AM run) made concrete: instead of accumulating unreviewed AI output, you accumulate *structured judgment about that output*.

---

## The architecture is natively buildable

The core components are simple:

```
PROJECTMEM architecture:
1. Event log (append-only JSONL file, typed events)
2. Projection function (deterministic: events → compact summary)
3. MCP server (serves summaries as tool context)
4. Pre-action gate (reads event log before tool calls)
```

This is implementable as a Jarvis skill in ~2-4 hours:
- Event log: `state/fiction-memory.jsonl` or per-manuscript files
- Projection: a skill that reads the log and produces a CC context block
- Pre-action gate: a hook that fires before Write/Edit tool calls and injects relevant prior-session history
- No third-party code needed; the pattern is the value

**Recommended:** Build the native version rather than adopting the third-party library. The pattern is the contribution, not the implementation.

---

## Suggested Jarvis native implementation

```yaml
# Sketch: fiction-memory skill
name: fiction-memory
trigger: session-start + pre-action hook
events:
  - issue: {text, chapter, characters}
  - attempt: {approach, result, chapter}
  - fix: {resolution, chapter}
  - decision: {choice, rationale, scope}
  - note: {observation, context}
pre-action gate:
  - fires before: Write, Edit tool calls
  - checks: relevant events for the target file/chapter
  - injects: "Prior context: [matching events]" into tool call prompt
```

**Next step:** Add to agent_suggestions.md as a buildable skill specification. Estimate: 2-4 hours to prototype, 1 week to dogfood across one manuscript.

---

## What to read

The full paper (arXiv:2606.12329) is worth reading for:
1. The typed event taxonomy (their choices are well-reasoned)
2. The pre-action gate implementation details (the specific prompt structure that works)
3. The 207-event study methodology (how to validate whether memory governance is actually helping)
