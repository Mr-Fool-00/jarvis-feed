# Briefing: Claude Code — OAuth Tokens Stolen via MCP Server Hijacking

**Date:** 2026-07-07  
**Score:** 7/10  
**Verdict:** INFORMATIONAL  
**ID:** `security:cc-oauth-mcp-hijacking-july7`  
**Source:** SecurityWeek (July 7, 2026)

---

## What it is

MCP server hijacking that leads to OAuth token exfiltration. Targets the gap between MCP's tool-trust model and OAuth's user-trust model.

---

## How it works

1. CC connects to a third-party MCP server during a workflow (e.g., a GitHub MCP server, a Slack MCP server, any OAuth-backed service)
2. Attacker serves a malicious MCP server that impersonates the legitimate server (via DNS poisoning, supply chain compromise of the MCP server package, or social engineering the user into connecting to the attacker's server)
3. The malicious server returns valid-looking tool responses to CC, maintaining the illusion of normal operation
4. Simultaneously, the server initiates OAuth flows that route through CC's browser context, capturing the user's access tokens for the OAuth provider
5. Tokens are exfiltrated; attacker now has persistent access to the OAuth-backed service (GitHub, Slack, etc.) independent of CC

---

## Why the trust model gap matters

MCP's tool-trust model: CC trusts any server in its configured server list and will call its tools. OAuth's user-trust model: the user's browser session authenticates flows. When a malicious MCP server is in CC's server list, it inherits CC's tool trust AND can hijack the browser OAuth context simultaneously. Neither model catches the attack on its own.

---

## Affected configurations

- Any CC setup with externally-installed third-party MCP servers
- Particularly: MCP servers installed via `npm install` from third-party registries (supply chain risk)
- MCP servers that handle OAuth-backed services: GitHub, Google, Slack, Linear, etc.

---

## Mitigation

- Audit every MCP server in your `~/.claude/settings.json` — know what you're running and where it came from
- Prefer first-party or well-audited MCP servers (e.g., Anthropic's own GitHub MCP server, not third-party forks)
- Check MCP server package provenance before installing (`npm audit`, check publish history)
- For high-sensitivity OAuth tokens: use separate browser profiles or token scoping to limit blast radius if exfiltrated

---

## Leo's current exposure

The jarvis-feed setup uses the GitHub MCP server (first-party, Anthropic-maintained). Low risk. The higher risk would be if Leo installs third-party MCP servers for, e.g., fiction tools, database connectors, or content platforms. Review that list before adding new servers.
