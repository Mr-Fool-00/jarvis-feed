# MCP & Agent Security Wave: 4 Attack Vectors (72 Hours) — 7/10

## What it is
Four separate security stories dropped July 27–29, all targeting MCP and Claude extension attack surfaces. Individually each is notable; together they signal that the ecosystem is catching up to MCP's rapid adoption and the attack surface is expanding fast.

**Attack #1 — FakeGit campaign (Help Net Security, July 21):**
Attackers poison AI agent recommendations with malicious GitHub repos. When Claude Code searches for libraries during a session, search result injection can redirect it to repos that install backdoors on clone. This is live and ongoing.

**Attack #2 — Microsoft Azure DevOps MCP Flaw (The Hacker News):**
Hidden comments in pull requests can hijack AI code review agents connected via MCP — silently redirecting their behavior with nothing visible in the PR UI.

**Attack #3 — Claude Cowork / Claude for Chrome flaws (The Hacker News):**
Claude Cowork has a VM escape bug (reads macOS files outside the sandbox). Claude for Chrome has a separate flaw where malicious browser extensions can hijack Claude and access Gmail, Docs, and Calendar.

**Attack #4 — Ruflo CVE-2026-59726 (CVSS 10.0):**
Open-source agent meta-harness (Ruflo) has an unauthenticated RCE via its unprotected MCP bridge endpoint. If installed and the bridge is exposed to any network, full compromise requires zero authentication.

## Why you'd want it (specific to your stack)
FakeGit is the most direct risk for your workflow: if you ever let CC search GitHub and auto-clone a suggested repo, you're in scope. Chrome extension flaws apply if you use Claude for Chrome. Ruflo only if you installed it (probably not, but worth confirming).

## Why I think it's worth your attention
This isn't isolated incidents — it's a pattern. Every new MCP integration = new attack vector. The community is converging on "your agent's environment is now an attack surface" as the dominant security concern for 2H 2026.

## What to do
Three concrete actions:
1. Add to CLAUDE.md: "ALWAYS verify any GitHub repo before cloning. Do not auto-install from search results."
2. Confirm you do not have Ruflo installed (`which ruflo` or check your tool installs).
3. If you use Claude for Chrome, treat it as compromised until a patched version is confirmed.

🔗 https://www.helpnetsecurity.com/2026/07/21/github-repos-malware-campaign-fakegit-ai-agents/
🔗 https://thehackernews.com/2026/07/microsoft-azure-devops-mcp-flaw-lets.html
🔗 https://thehackernews.com/2026/07/claude-cowork-flaw-could-let-ai-agent.html
🔗 https://thehackernews.com/2026/07/claude-for-chrome-flaw-lets-other.html
🔗 https://adversa.ai/blog/top-mcp-security-resources-july-2026/
