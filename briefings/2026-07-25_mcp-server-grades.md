# HN: Teng Li Graded 36 MCP Servers on Agent Usability — A Third Got D or F — 7/10

**Date:** 2026-07-25
**Source URL:** https://tengli.dev/posts/mcp-servers-failing-agents.html
**Score:** 7/10
**Category:** Practitioner tool / calibration data — not third-party code

---

## What it is

Teng Li built `mcpgrade`: it synthesizes realistic tasks, shows a model the full tool catalog, then measures tool selection accuracy and argument correctness. Graded 36 popular MCP servers. Results: 42% scored A, 31% scored D or F. The dominant failure is rule D004: parameters with no descriptions. Firecrawl MCP: F/57, 134 errors. Archived Slack reference server: A/97 — someone wrote every parameter description by hand. Notion (official): D/62. MongoDB (official): D/66. Maintenance velocity does not correlate with agent usability. The fix for most failing servers is writing, not coding. Full leaderboard at mcpgrade.com.

---

## Why you'd want it (specific to your stack)

Any MCP Jarvis uses — or any MCP built for Jarvis — lives and dies by parameter descriptions.

The D004 rule is simple: if a parameter has no description, a model guessing what to pass has no signal. It either skips the tool (tool selection failure) or passes garbage arguments (argument correctness failure). Either way the tool call fails silently from the model's perspective.

The calibration data here is directly useful before building any Jarvis MCP. Every parameter needs a description. That's the whole bar. It's writing work, not coding work.

The grading tool itself (`mcpgrade`) is worth running against any MCP you're evaluating before adoption — it gives you a score and a breakdown of exactly which rules failed, so you know whether a failing server is a one-hour fix (write descriptions) or a structural problem.

---

## Why I think it's worth your attention

Jarvis's deja-vu memory MCP briefing (same run) is directly affected by this finding. If deja-vu's four MCP tools (`recall`, `recall_context`, `blame`, `remember`) have weak parameter descriptions, a model will fumble them. That's an easy check before investing in native implementation.

More broadly: the Teng Li finding explains why MCP adoption feels unreliable in practice. It's not the protocol. It's the docs inside the server. This is a fixable problem, and now you have a benchmark to measure it.

---

## What to do

No build. Calibration data for any future MCP work.

1. Before adopting any MCP, run `mcpgrade` against it (or check mcpgrade.com if it's already been graded).
2. When building native Jarvis MCPs: write every parameter description, no exceptions. D004 is the whole exam.
3. Run `mcpgrade` against deja-vu (if in scope) before deciding whether to use it or build native.

🔗 https://tengli.dev/posts/mcp-servers-failing-agents.html
