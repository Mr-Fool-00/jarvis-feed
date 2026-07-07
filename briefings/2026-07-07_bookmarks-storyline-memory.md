# Briefing: BOOKMARKS — selective storyline memory for fiction agents

**Date:** 2026-07-07  
**Score:** 7/10  
**Category:** BUILDABLE — PreCompact hook or /story-checkpoint skill  
**ID:** `arxiv:2605.14169-bookmarks-storyline-memory`

---

## What it is

arXiv:2605.14169, "BOOKMARKS: Efficient Active Storyline Memory for Role-playing Agents" (May 2026).

The paper proposes replacing full-context retention with selective bookmarking: the agent identifies narratively significant moments — new character introductions, relationship shifts, key decisions, world-state changes — and writes structured summaries of those moments to a persistent store. Everything else is allowed to fade. The agent queries this bookmark store during generation rather than holding all prior context in window.

Empirically: bookmark-based agents maintained narrative coherence on 40k+ token stories while reducing active context by ~70%. The full paper is at https://arxiv.org/abs/2605.14169.

## Why Leo would want this

Your long-form fiction pipeline has a known context-drift problem: by chapter 8-10 of a novel-length run, CC forgets character voice, prior plot beats, and world-state details that weren't in the most recent chapters. The BOOKMARKS approach solves exactly this, without just cramming everything into an ever-growing system prompt.

The architectural fit is clean: a PreCompact hook runs before CC compresses context, identifies plot-critical beats via an LLM call, writes them to `memory/story-checkpoints.json` (or similar), and those checkpoints get injected back into every subsequent prompt as a structured anchor. The story never loses track of who knows what, who's where, or what happened.

## Why Jarvis flagged it

This isn't a general memory paper — it's specifically about roleplay/fiction agent memory, and it's not yet in your seen feed. The skill shape is well-defined enough to build in one session. It addresses Leo's stated bottleneck (fiction pipeline context drift) with a paper-backed approach rather than ad hoc prompting.

## What to do

1. Read the abstract + Section 3 (Bookmark Extraction) of https://arxiv.org/abs/2605.14169 — 15 minutes
2. The core extraction prompt they use classifies events into: Character Introduction, Relationship Change, Key Decision, World Event, Protagonist Development
3. Skill shape:
   - **PreCompact hook**: `hooks/story-compactor.py` — before CC compresses, runs an LLM call with the current context to extract and write bookmarks
   - **Checkpoint file**: `memory/story-checkpoints.json` — structured store of bookmark objects
   - **Inject hook**: `hooks/story-context-injector.py` — prepends relevant bookmarks at start of each new session
4. Say the word and we can build this in the next run. One session, probably 2-3 hours of agent time.

*Source: https://arxiv.org/abs/2605.14169 — May 2026*
