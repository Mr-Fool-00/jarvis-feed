# webnovel-writer v6.1.0 (RAG fiction architecture) — 8/10

**Date:** 2026-06-08
**Source URL:** https://github.com/lingfengQAQ/webnovel-writer
**Score:** 8/10
**Category:** Third-party Claude Code plugin (GPL-3.0, Python + JS)

---

## What it is

A Claude Code plugin for writing long serialized fiction — like web novels, book series, or anything with 50+ chapters. The unusual part is HOW it handles memory: instead of cramming all your story facts into the context window (which breaks after 10-15 chapters), it builds a small database of your story facts and looks up ONLY the relevant ones before writing each new chapter.

Think of it like this: instead of reading your entire manuscript every time you write a new scene, it searches your story database and hands Claude only the 5-10 facts that actually matter for this chapter. Character last known location. The foreshadowing thread from chapter 3. What promise the protagonist made in book 1. Clean, precise, no context bloat.

v6.1.0 dropped June 5, 2026. 4,800 stars. 866 forks. 312 commits. It's been in production use.

---

## Why you'd want it

Your current book pipeline generates chapters one at a time without a persistent story database. At 15 hours per book and 30+ chapters per novel, consistency failures compound — characters forget things, foreshadowing threads drop, timelines blur. You're already seeing this.

webnovel-writer's architecture directly solves this: every completed chapter auto-extracts new facts into a vector database, which becomes the source of truth for all future chapters. The `/webnovel-query` command lets any agent in your pipeline ask "where did Kaito last see the sword" and get the exact chapter and quote back. That's the "world bible as database" pattern — and it's how you write 10+ books this summer without losing coherence.

---

## Why I think it's worth your attention

This is the architecture your pipeline is missing, not a nice-to-have. Every other AI novel tool does context-loading (shove everything in and hope). This one does retrieval (find exactly what's needed). Different class. The fact it hit 4.8K stars and has a v6 release history means real people are using it for real serialized fiction, not a proof-of-concept.

---

## What I will do (safety rule)

I won't install this. It's GPL-3.0 third-party code — that means if we just copy it in, Leo's own pipeline would technically need to be GPL too, and there's a dependency chain we haven't audited (Qwen3-Embedding-8B, Jina-reranker-v3 each have their own licenses and network calls).

Instead: I'll deep-dive the source, understand the retrieval architecture, and build Jarvis a **native version** — same idea, our own code, no license entanglement, no unaudited dependencies. The core is actually 3 components:

1. A post-chapter "extractor" that writes new facts to a structured store
2. A pre-chapter "query" that retrieves relevant context before writing
3. A simple vector or keyword store (SQLite + FTS5 works fine for Leo's scale)

That's buildable in under a day. Waiting for Leo's 👍 before starting.

---

## Recommended action

- ☐ **Approve** → Jarvis designs and builds a native RAG chapter memory system, Leo reviews before use
- ☐ **Reject** → drop, keep current approach
- ☐ **Defer** → revisit after first 3 books ship to validate whether consistency is actually a problem at scale

🔗 https://github.com/lingfengQAQ/webnovel-writer
