# Addy Osmani: Claude Code Agent Teams — Swarm Coordination Pattern

**Score:** 8/10 · **Run:** 2026-06-28 PM · **Source:** addyosmani.com/blog/claude-code-agent-teams/

---

## What it is

Addy Osmani published a concrete pattern for coordinating parallel Claude Code agents without state conflicts. The architecture: a **lead coordinator agent** dispatches work to **N parallel sub-agents** that operate against a **shared task list** using **file locking** to prevent write conflicts. Each sub-agent claims a locked slice of work from the shared registry; the coordinator waits for all claims to resolve before proceeding to synthesis.

The shift this formalizes: from the "conductor model" (you direct agents one at a time, interactively) to the "orchestrator model" (coordinator delegates to N agents simultaneously, monitors progress, recombines outputs).

Key mechanism: shared task list + per-task file locks. When Agent A claims `chapter-03`, it locks that slot. Agents B and C skip it and claim different slots. The coordinator polls until all slots are resolved, then synthesizes.

---

## Why you'd want it

Right now, running 3 parallel chapter-writing agents in Leo's `/book-pipeline` would cause them to stomp each other's state — no safe handoff, no coordination, no synthesis step. The SPOQ paper (also this run, arxiv 2606.03115) confirms that quality-control failures concentrate at handoff boundaries in multi-agent systems.

Osmani's pattern is the reference design for fixing exactly that. It's not theoretical — he documents it as a working architecture, with the shared-task-list + file-claim primitive as the concrete implementation.

---

## Why I think it's worth your attention

Addy Osmani is the highest-signal Claude Code practitioner blogger. His prior posts (Claude Code Best Practices, The Art of Strategic Laziness, etc.) have been build-quality references, not hype. This one arrived the same run as the companion reference — Hidekazu-Konishi's Extension Layer Decision Guide — which maps exactly which CC abstractions (Skills, Hooks, Subagents, Plugins) belong in each part of this architecture. Both arrived together. That's the full design doc for `/book-pipeline` V2.

Also: Dicklesworthstone/claude_code_agent_farm (847 stars, item 5 this run) independently validates the lock-based approach with adaptive stagger timing. Three sources, same pattern. It's the convergent answer.

---

## What to do

1. **Read the article**: addyosmani.com/blog/claude-code-agent-teams/ — should be ~10 min. Look specifically at how the shared task list is structured (JSON registry vs flat file vs directory-based) and what happens when a sub-agent crashes mid-claim (stale lock detection).

2. **Bookmark alongside**: hidekazu-konishi.com/entry/claude_code_extension_layers_decision_guide.html — this maps which CC extension type to use for each piece of the architecture.

3. **Say the word**: If you want Jarvis to build a native `/book-pipeline` V2 coordinator skill using this pattern, just say so. The design is clear enough to build now. Estimated output: a Skills-based coordinator that manages a shared chapter registry, dispatches parallel Subagent chapter-writers with file-lock claims, and synthesizes on completion. No third-party code — native CC.

---

*Jarvis Discovery Loop · 2026-06-28 PM run*
