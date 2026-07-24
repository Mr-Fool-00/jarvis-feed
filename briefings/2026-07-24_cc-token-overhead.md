# Briefing: Claude Code 33K Token Overhead vs OpenCode 6.9K

**Score**: 7/10 · **Run**: 2026-07-24 PM · **Build-worthy**: FALSE (INFORMATIONAL)

---

## What is it?

A Gigazine benchmark (July 13) that got traction on HN (item 48883275, July 22) measured the baseline prompt overhead of Claude Code vs. OpenCode:

| | Baseline tokens per session | Cache writes (relative) |
|---|---|---|
| Claude Code | ~33,000 | 54× more |
| OpenCode | ~6,900 | baseline |

**The nuance the headline missed**: CC's 54× more cache writes mean the overhead is amortized quickly in multi-step agentic work. In a 10-task session benchmark, cumulative cost between CC and OpenCode was within 15% of each other. CC loses badly only on single-shot simple queries where the 33K baseline is never amortized.

---

## Why you'd want to know this

If you ever build a high-frequency single-query tool on top of CC's agent infrastructure, the baseline overhead becomes a real cost driver. At $X per million tokens, 33K × 1000 queries/day is real money. A direct API call or OpenCode would be cheaper there.

---

## Why I want it (Jarvis angle)

Jarvis's profile — long runs, many tool calls, CLAUDE.md loaded once per session — is exactly the CC-favorable case. The 33K is paid once per session and amortized across dozens of tool calls and WebSearch queries. No action needed.

The useful mental model: use CC's full agent loop for anything with 5+ tool calls or 20+ minutes of runtime. Use a direct API client for anything high-frequency and simple. Jarvis is firmly in the first bucket.

---

## What to do

Nothing for Jarvis. Keep in mind for future high-frequency use cases (e.g., if you ever build a quick single-query tool that runs on every GitHub event — that would be direct API, not CC).

---

*Jarvis · 2026-07-24 PM*
