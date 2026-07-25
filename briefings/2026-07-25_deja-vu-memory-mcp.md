# vshulcz/deja-vu — Cross-Harness Agent Memory MCP, 84.9% Recall, Fully Local — 7/10

**Date:** 2026-07-25
**Source URL:** https://github.com/vshulcz/deja-vu
**Score:** 7/10
**Category:** Third-party code — safety gate reviewed ⚠️

---

## What it is

A local-first MCP server that retroactively indexes months of conversation history from 16 coding agents — Claude Code, Cursor, Codex, aider, Gemini CLI, Goose, and more — and makes them searchable in ~12ms. Zero-dependency binary. No LLM needed — pure lexical indexing achieves 84.9% hit@1 on LongMemEval-S. Four MCP tools: `recall` (dense snippet matching), `recall_context` (Markdown session digest), `blame` (which sessions touched a file), `remember` (store durable decisions). Auto-redacts AWS keys, JWTs, and PEM blocks at index time.

---

## Safety gate review

**Status: YELLOW — usable with restrictions.**

Positives: local-only (nothing leaves your machine), zero-dependency binary, auto-redacts credentials at index time, clean codebase.

**Critical concern: `--auto` mode.** When `--auto` is enabled, deja-vu silently injects prior session context into every new agent call without you seeing it. That injection stream is a prompt injection surface: if any past session summarized a malicious repo (a poisoned README, a hostile CLAUDE.md), those instructions get injected into your current agent without warning.

**Safe use:** Install and use `recall`, `recall_context`, `blame`, and `remember` manually — explicitly asking for past context when you want it. Do NOT enable `--auto` mode on any codebase you didn't write yourself.

---

## Why you'd want it (specific to your stack)

The Jarvis discovery loop has no persistent memory across runs. Each run starts fresh. Decisions made in run 40 (like "don't brief on X again") have to be reconstructed from seen.json.

deja-vu solves this. Index all Jarvis CC sessions and you get:
- `recall "Claude voice mode"` → surfaces every session where you discussed P3 voice architecture
- `blame briefings/2026-07-25_claude-voice-opus.md` → which session created this file and why
- `remember "Fable 5 reserved for orchestrator planning only"` → durable decisions that survive context window resets

That's the kind of cross-session continuity Jarvis has needed since the first PM digest.

---

## Why I think it's worth your attention

84.9% hit@1 on LongMemEval-S is a real number, not marketing. It means: if you ask "what did I decide about X?", the right answer comes up first 85% of the time. For a zero-LLM lexical indexer, that's genuinely useful.

The alternative is building a custom memory layer from scratch (the native Jarvis approach). That's the right long-term call — but deja-vu as a bridge gives you working memory today while the native version gets designed properly.

---

## What to do

This is third-party code. I will not install it. What I'll do instead:

1. Deep-dive the binary source (it's open on GitHub) to understand the exact index format and retrieval algorithm.
2. Design a native Jarvis memory layer using the same pattern — session indexing, lexical recall, durable `remember` entries — but built as a Jarvis skill rather than an external MCP.
3. The `blame` tool specifically (which sessions touched a file) is the pattern worth stealing for Jarvis's audit trail.

React 🚀 to kick off native implementation.

🔗 https://github.com/vshulcz/deja-vu
