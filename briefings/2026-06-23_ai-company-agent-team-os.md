# Briefing: CronusL-1141/AI-company — CC-Native Multi-Agent Team OS

**Date:** 2026-06-23  
**Score:** 7/10  
**Type:** B — Third-party code (safety gate applied)  
**Channel:** #improvements

---

## Safety gate result: DEEP-DIVE COMPLETED ✅

- **Stars:** 107 (low — borderline for safety gate)  
- **License:** MIT  
- **External API calls:** Zero by default. Optional GitHub/Slack/Linear integrations are explicitly opt-in.  
- **Safety layer:** 48+ built-in rules, 7 dangerous pattern detections, PII warnings, file locks, agent trust scoring  
- **Code review:** Well-structured, 22 modules, no unusual postinstall scripts, no hidden remote fetches  
- **Safety verdict:** Star count is low but safety architecture is unusually mature. **DO NOT install. Extract patterns only.**

---

## What it is

A multi-agent team OS built entirely on Claude Code — no LangChain, no AutoGen, no external LLM dependencies. Runs within the CC plan. 107 MCP tools across 22 modules.

**Key subsystems:**
- **Agent management:** 40+ agent templates with roles, capabilities, trust scores
- **Meeting system:** Structured protocol — agenda → discussion → voting → minutes → action items. Agents don't just chat; they run a meeting.
- **Shared task wall:** All agents write to a common task board. Progress visible across sessions.
- **Debate system:** Dedicated module for agents to argue positions, surface disagreement, and converge on a decision with logged reasoning.
- **React dashboard:** Live view of agent activity, task status, meeting state.
- **10 lifecycle hooks + 7 pipeline workflows**

---

## Why it matters for your setup

The `/council` slash-command you already have is a primitive ancestor of this. This shows the fully fleshed-out version.

**The critical gap it exposes in your current setup:** Your multi-agent runs don't persist state across sessions. When a Council session ends, the agents' conclusions evaporate unless you manually summarize them. AI-company solves this with its shared task wall + meeting minutes format.

**Two patterns worth extracting:**

**1. Meeting minutes format for Council runs:**
Instead of a free-form Council discussion, structure it as:
- AGENDA: what are we deciding?
- DISCUSSION: each agent states position + evidence (one pass)
- RESOLUTION: majority or facilitator calls it
- MINUTES: 3-5 bullet summary committed to a file

The minutes become the persistent artifact that the next session can read. This is the pattern your current Council is missing.

**2. Shared task wall:**
A markdown file (`tasks.md` or `project-wall.md`) that your writing pipeline agents all write to and read from. Each agent appends its output status and blockers. The orchestrator reads it to decide next steps. No complex state machine — just a shared file with a simple format.

**Why not the meeting system?** The full 107-tool OS is overkill for your use case. You don't need 40 agent templates or a React dashboard. What you need is (a) structured Council output and (b) a cross-session task wall for the writing pipeline.

---

## What to build (your version, not an install)

1. **Council minutes format** — update `/council` to produce structured output: AGENDA / DISCUSSION (per agent) / RESOLUTION / ACTION ITEMS. Commit minutes to `council-minutes/YYYY-MM-DD.md` after each run.

2. **Project task wall** — `state/writing-pipeline.md` that tracks current book, chapter in progress, last completed chapter, open blockers, and next agent action. Writing agents read and update this file.

Leo approval needed before building. Small build — 1 session each.

---

## Sources

- https://github.com/CronusL-1141/AI-company (MIT, June 2026)

---

**Build verdict:** BUILDABLE — Extract two patterns: Council meeting-minutes format + shared task wall for writing pipeline. Full 107-tool OS is not the goal. **Awaiting Leo approval.**
