# lingfengQAQ/webnovel-writer v6.1.0 — 8/10

**Date:** 2026-06-07
**Source URL:** https://github.com/lingfengQAQ/webnovel-writer
**Score:** 8/10
**Category:** Third-party Claude Code plugin (safety gate applies)

---

## What it is

A Claude Code plugin for writing long-form serial fiction — web novels, chapter series, multi-book projects. It solves the hardest problem in AI-assisted fiction: keeping the AI from forgetting characters, plot threads, and world-building details across hundreds of chapters.

Instead of dumping your whole story into the context window and hoping the AI remembers, it runs a real retrieval system — a database that stores every established fact, and pulls only what's relevant before writing each new chapter. 4.8K stars, 866 forks, 312 commits, v6.1.0 released June 5. Active and serious.

---

## Why you'd want it (specific to your stack)

You're trying to write 10-15 books this summer. Your biggest obstacle after throughput speed is consistency — an AI that forgets what a character's voice sounds like in chapter 3 when you're writing chapter 47, or contradicts established world-rules 80K words in.

webnovel-writer solves exactly this. Here's how it works for your pipeline:

- Every chapter you write gets "committed" as a transaction: facts extracted, character motivations registered, timeline updated in a vector database.
- When writing chapter 47, the plugin does a hybrid search (keyword matching + semantic search) across all 46 prior chapters and retrieves only the relevant context — not the whole manuscript, just what matters for this scene.
- An 8-command skill set handles init → plan → write → review → query → dashboard. The `/webnovel-query` command lets you ask things like "where is [character] currently?" or "what foreshadowing threads are unresolved?" and get accurate answers sourced from the committed story database.
- Supports up to 2-million-character projects (roughly 5-6 full-length novels in a single project).

Your Fate-Anchor, shadow-emperor, and primal-survival series — any world with tracked factions, magic systems, and multi-arc plots — would directly benefit from the consistency layer this adds.

---

## Why I think it's worth your attention

This is the architecture gap in your current setup. Right now your chapter generation is isolated — each chapter starts fresh. The RAG-based Story System is how you go from "AI writes chapters" to "AI writes a coherent series." The 37 built-in genre templates include isekai and cultivation fiction specifically. And the version history (v6.1.0 means they've been at this for a while) means the rough edges are mostly worked out.

The `/webnovel-doctor` command alone is useful — it runs health checks on your story database so you can catch data drift before it causes plot holes.

---

## What I will do (safety rule)

I won't install this. Two things worth knowing: it's GPL-3.0 (copyleft — if you publish derivative code, it may need to be open-source; internal tools are fine), and it makes external API calls to an embedding service (Qwen3-Embedding-8B default, any OpenAI-compatible endpoint works, so you can point it at a self-hosted model).

When you approve, here's what I'll build:

**Native Jarvis Story System** — inspired by webnovel-writer but Leo-owned code:
1. **`/story-init`** — creates `story_state.json`, character registry, world-rules file, foreshadowing ledger
2. **`/story-write [chapter N]`** — retrieves relevant prior context via BM25 (fast, no external API) before writing; commits facts after
3. **`/story-query [question]`** — searches committed story database, returns sourced answers
4. **`/story-review [chapter N]`** — multi-dimensional review: continuity, pacing, voice consistency, plot hook status

No vector DB dependency for the initial build (BM25 only — zero external API calls). Can upgrade to embedding-based retrieval later if BM25 misses too much. This sidesteps the GPL and external-API concerns entirely while keeping the core pattern.

React below or in #improvements:
- ☐ Approve → Jarvis builds native Story System skill (1-2 sessions)
- ☐ Reject → drop, won't surface again
- ☐ Defer → surface again after first book shipped

🔗 https://github.com/lingfengQAQ/webnovel-writer
