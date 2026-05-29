# Aguara v0.19 — Security Scanner for AI Agent Skills — 8/10

## What it is
A security scanner specifically built for AI agent skills and MCP servers. You point it at a SKILL.md file or an MCP config and it tells you whether anything malicious is hiding inside it — before you install it. No internet connection needed, no API key, no cloud account. Just a single Go binary that runs 219 detection rules offline. It catches things like hidden prompt injection commands, code that tries to send your files to an external server, OAuth token theft patterns, and supply-chain attacks disguised in dependencies. Fresh release (v0.19.0) was yesterday, May 26.

## Why you'd want it (specific to your stack)
The runbook I run says "never install third-party skills blindly." Right now that rule is enforced by me doing a manual code review before briefing you on anything. That's slow and I can miss things. Aguara runs 219 automated rules in under a second and catches the stuff that's easy to miss in a fast read. In March 2026, the LiteLLM supply chain attack used malicious Python hooks to exfiltrate credentials silently. Aguara would have caught that pattern. Every time you're thinking about installing one of the skills I brief you on — claude-memory-compiler, claude-mem, howells/fiction — running Aguara first takes 5 seconds and either clears it or flags the risk. This is the missing link in the safety gate.

## Why I think it's worth your attention
It also has an MCP server version (aguara-mcp) — so Claude can scan a skill before self-installing it during an agentic workflow. If you ever build a "Jarvis installs approved skills automatically" feature, security scanning can happen as part of that flow without manual intervention.

## What to do
Type B item — it's code that would run on your system. I've done an initial read: Go binary, compile from source, no network calls during scanning. No obvious red flags. But I want to do a full deep-dive on the hook logic and binary release signing before recommending you run it.

React 🚀 to move to build queue — I'll finish the deep-dive and propose integration into the Jarvis skill-install workflow.
React 👎 to drop.

🔗 https://github.com/garagon/aguara
