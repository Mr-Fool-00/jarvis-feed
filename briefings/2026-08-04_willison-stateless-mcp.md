# Briefing: Simon Willison — Stateless MCP Recaptures His Interest (mcp-explorer + datasette-mcp)

**Date:** 2026-08-04 · **Score:** 8/10 · **Build verdict:** INFORMATIONAL  
**Source:** simonwillison.net · https://simonwillison.net/2026/Jul/31/stateless-mcp/  
**Companion item:** Simon Willison TIL (July 29) — https://simonwillison.net/2026/Jul/29/mcp-in-claude-and-chatgpt/  
**Digest item:** #2 of 2026-08-04 PM

---

## What Willison built and why it matters

Simon Willison called the 2026-07-28 stateless MCP spec "the most significant change to MCP since remote servers launched" — and backed it up by shipping two tools within 3 days:

### mcp-explorer
- An MCP server introspection tool — "Postman for MCP"
- Connects to any MCP server and lets you browse its tools, resources, and prompts interactively
- Under the old spec, every inspection session required a session-pinning initialization handshake; the stateless spec makes each request self-contained, so mcp-explorer can inspect servers without maintaining a session
- **Install/run:** `uvx mcp-explorer` (no pip install required; runs from uv)
- **Immediate use case:** debugging Jarvis's GitHub, Gmail, and other MCP connections. Instead of reading raw protocol logs, you can see exactly what tools a server exposes, their schemas, and test them interactively

### datasette-mcp
- An MCP server for Datasette that exposes SQL query execution and schema inspection as stateless HTTP tools
- Claude can query any SQLite database via a self-contained HTTP POST — no server process to keep alive
- Willison noted this was his "fourth time" trying to build datasette-mcp; previous attempts were blocked by the stateful handshake overhead. The stateless spec made it practical.
- Exposes 3 tools: `execute_sql`, `get_schema`, `list_tables`
- **Relevance to fiction pipeline:** if you ever want Claude to query a SQLite world-bible database (character relationships, timeline events, locations, faction structures), datasette-mcp is a near-zero-friction way to wire that up as an MCP tool

### llm-mcp-client (also released same day)
- A companion tool from Willison that lets you use MCP servers from the `llm` CLI — the same day as datasette-mcp and mcp-explorer
- One man shipping three related tools on the same day is a signal that the stateless spec materially lowered the implementation cost

---

## The stateless spec shift in plain terms

Under the old spec:
1. Client sends `initialize` with capabilities
2. Server responds with its capabilities, establishing a session
3. All subsequent tool calls reference that session
4. Clients and servers had to maintain state — hard to run serverlessly, requires session stickiness in load balancers

Under the 2026-07-28 stateless spec:
1. Every request is a self-contained HTTP POST
2. No `initialize` handshake, no `Mcp-Session-Id` header
3. The same endpoint works in both Claude Desktop and ChatGPT (Willison's TIL confirmed this)
4. Server-side: can run as a simple stateless HTTP function — Lambda, Cloudflare Worker, Fly.io, anything

---

## Build candidate: mcp-inspector for Jarvis

The mcp-explorer pattern — a tool that can connect to and introspect any MCP server — is worth building natively for Jarvis. It would let future runs verify that connected MCP servers are responding correctly and expose their current tool catalogs. Not urgent; note for future Jarvis tooling review.

---

*Jarvis · auto-briefing · 2026-08-04 PM*
