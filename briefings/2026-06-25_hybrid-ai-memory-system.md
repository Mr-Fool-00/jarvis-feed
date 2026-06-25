# MindStudio: Hybrid AI Memory System for Claude Code — 7/10

**Date:** 2026-06-25
**Source URL:** https://www.mindstudio.ai/blog/hybrid-ai-memory-system-claude-code-storage-injection-recall
**Score:** 7/10
**Category:** Architectural pattern (tool-agnostic; MindStudio tools used as illustration)

---

## What it is

A guide to giving Claude Code persistent memory across sessions using three layers: a **storage tool** (Hermes) that writes session observations to files as you work, a **semantic recall layer** (MemSearch — 2.1k stars, pip-installable, no API key needed, uses ONNX embeddings locally) that lets you find past decisions by meaning not keyword, and **Claude Code hooks** in `.claude/settings.json` that fire at session start and inject the most relevant memories before Claude types its first word.

The key insight: `CLAUDE.md` can be deprioritized by the model, but a `SessionStart` hook is deterministic — it fires first, every time, and Claude cannot skip it.

## Why you'd want it (specific to your stack)

Jarvis loses all context every 12 hours. It re-reads the full AGENT_RUNBOOK.md and JARVIS_PERSONA.md cold each run because there's no memory of what worked, what scored high, what Leo reacted to. A `SessionStart` hook that injects a condensed "prior-run state" (recent scores, reaction patterns, items already briefed, ranking adjustments Leo made) would mean each run starts smarter, not from zero. The `StopHook` counterpart would auto-write a session summary so the next run inherits it.

This also directly applies to your longform writing pipeline: a scene-level memory store that recalls craft decisions from prior chapters ("you used X technique in chapter 3 — continue or break?") could replace the manual context-resetting you do now.

## Why I think it's worth your attention

You already have hooks wired in jarvis-feed's `.claude/settings.json`. The architecture is a 30-line shell script away from working. MindStudio's proprietary tools (Hermes, their hosted MemSearch) are optional — the pattern works with plain files and MemSearch's pip package.

## What to do

Read the MindStudio article to understand the 6-layer memory taxonomy, then decide which layers make sense to bolt onto jarvis-feed first. My recommendation: start with the `StopHook` → write-session-summary path (no deps beyond shell + git) before adding the vector recall layer. I can draft the hook scripts if you approve.

🔗 https://www.mindstudio.ai/blog/hybrid-ai-memory-system-claude-code-storage-injection-recall

---

## Build-our-own sketch (if approved)

**Layer 1 — StopHook session summarizer (no new deps):**
A shell script triggered by `.claude/settings.json` `StopHook` that appends a compact JSON record to `state/session_log.jsonl`: timestamp, items-surfaced, top scores, reactions seen, any ranking adjustments made. Pure files, no vector DB. Next run's SessionStart hook reads the last 3 entries and formats them as a context injection block.

**Layer 2 — SessionStart injector:**
A shell script that reads `state/session_log.jsonl`, formats the top-N relevant entries (most recent + highest-reacted items), writes them to a temp `identity-core.md`, and outputs that file's content so Claude Code sees it at context open. This is the "deterministic memory" layer that replaces re-reading the full AGENT_RUNBOOK.md from scratch.

**Layer 3 — Semantic recall (optional, add later):**
If session logs grow large (>30 entries), add MemSearch (`pip install memsearch[onnx]`) to do hybrid retrieval: embed each session log entry, query by current-run context at session start, inject only the top-3 most semantically relevant past sessions rather than the 3 most recent. This is the actual MindStudio pattern — but it's overkill until Layer 1+2 are working.

**What I won't do:**
Install Hermes, MindStudio's proprietary storage tool, or any part of MindStudio's managed platform. The Leo-owned version uses shell + git + (optionally) MemSearch pip package.

---

## Decision for Leo

- ☐ **Approve** → I build Layer 1 (StopHook summarizer) in next run, no deps added, fully reversible
- ☐ **Approve + Layer 2** → I build both hook scripts + identity-core.md injection in next run
- ☐ **Defer** → re-evaluate when jarvis-feed session logs are large enough to need it
- ☐ **Reject** → CLAUDE.md + manual runbook re-read is fine; drop this category
