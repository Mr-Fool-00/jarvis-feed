# Briefing: Blackboard Architecture for Multi-Agent Fiction Pipelines

**Date:** 2026-07-02 · **Source:** arXiv:2507.01701 · **Score:** 8/10 · **Digest item:** 2026-07-02 AM, Item 2

---

## What is it?

A new coordination pattern for multi-agent pipelines, published by CMU and Stanford researchers in July 2026. Instead of passing context between agents serially (agent A summarizes what it learned → passes to agent B → B summarizes → passes to C), all agents share a single structured document — the **blackboard** — that they all read from and write to.

The analogy: a shared whiteboard in a team room. Everyone in the room can see the full current state. Nobody has to repeat what they said in the last meeting.

---

## The problem it solves

Leo's current fiction pipeline passes context linearly:

```
Research agent → writes summary → Outline agent reads summary → writes outline →
Prose agent reads outline → writes chapter
```

Each handoff compresses and slightly mutates the state from the step before. By the time the prose agent runs, it has a filtered, summarized version of the research — not the research itself. This is the **game of telephone** problem, and it's why AI fiction agents tend to drift from the original intent chapter by chapter.

The CMU/Stanford paper measured this: pipelines using serial context passing had 12–31% higher plot contradiction rates than pipelines using a shared blackboard, across two long-form fiction benchmarks.

---

## How the blackboard pattern works

```
                ┌──────────────────────────────┐
                │   BLACKBOARD (shared doc)    │
                │                              │
                │ ## Characters                │
                │ ## Plot decisions            │
                │ ## World facts               │
                │ ## Unresolved threads        │
                │ ## Chapter-by-chapter state  │
                └──────────────────────────────┘
                       ↑ read/write ↑
          ┌────────────┤            ├────────────┐
    Research agent   Outline agent           Prose agent
```

Each agent:
1. Reads the sections of the blackboard it needs
2. Does its work
3. Writes its output **back to the blackboard** in the sections it owns

The orchestrator coordinates **which agent runs when**, but doesn't carry any content — just sequencing.

---

## Why this matters for Leo's stack specifically

Leo's fiction pipeline already uses a `CONTEXT.md` file as a primitive version of this pattern. The blackboard architecture is the formalized, multi-agent version. Key improvements over the current `CONTEXT.md` approach:

1. **Explicit section contracts:** Each agent declares which sections it reads and writes. No section gets accidentally overwritten by an agent that wasn't supposed to touch it.

2. **Inspectable at any point:** Leo can open the blackboard between any two agent runs, edit it, add notes, and the next agent picks up the changes. This is already how `CONTEXT.md` works — the blackboard just makes it structured and typed.

3. **Pairs cleanly with TOKI (bitemporal memory):** TOKI (arXiv:2606.06240, also this digest run) adds timestamps to each blackboard entry, so when a revision creates a contradiction, the contradiction detection system can flag it before the prose agent runs.

---

## What a Jarvis-built implementation would look like

**Phase 1 (1 session):** Add a structured `BLACKBOARD.md` schema to Leo's fiction pipeline project. Sections:

```markdown
## Characters
<!-- Agent: character-sim writes; prose-agent reads -->
...

## Plot decisions  
<!-- Agent: outline writes; prose-agent reads -->
...

## World facts
<!-- Agent: research writes; all agents read -->
...

## Unresolved threads
<!-- Agent: any writes; orchestrator reads -->
...

## Chapter log
<!-- Agent: prose-agent writes after each chapter -->
...
```

**Phase 2 (1 session):** Update each agent's CLAUDE.md or system prompt to include its read/write contract. Each agent gets explicit instructions: "Read sections X, Y. Write to section Z. Do not modify other sections."

**Phase 3 (optional):** Add a lightweight contradiction-detection pass that runs after each agent write, comparing new entries against existing character/world facts. This is where TOKI's bitemporal indexing would plug in.

---

## Effort estimate

- Phase 1: ~30 minutes to draft the schema, ~1 CC session to refine
- Phase 2: ~1 CC session to update each agent's instructions and test
- Phase 3: ~2–3 CC sessions (more complex, requires embedding/comparison layer)

Phases 1 and 2 are low-risk, low-effort, and deliver most of the value. Phase 3 is where the research paper's results actually come from — but it's not required to get the coordination benefit.

---

## What Leo needs to approve

This is a **design pattern and architecture change** — not a third-party package install. The work involves:
- Creating a `BLACKBOARD.md` schema file in Leo's fiction pipeline project
- Updating existing agent CLAUDE.md files / system prompts
- No new npm/pip packages
- No external dependencies

Per Jarvis rules, Jarvis would build its own implementation inspired by the paper, not copy any paper code (the paper itself is theoretical — no reference implementation exists yet).

**Recommended next step:** Leo approves the Phase 1 schema draft, and Jarvis kicks off a fiction pipeline session to draft `BLACKBOARD.md` against Leo's current project structure.

---

## Links

- Paper: https://arxiv.org/abs/2507.01701
- Related: TOKI (arXiv:2606.06240) — bitemporal contradiction resolution
- Related: StoryBox (arXiv:2510.11618) — character simulation before prose
- Digest: digests/2026-07-02_AM.md, Item 2
