# Claude Forge — "Oh-My-Zsh for Claude Code" — 7/10

## What it is
Claude Forge is a plugin framework for Claude Code that bundles 11 AI agents, 40 commands, 15+ skills, and 6-layer security hooks into a 5-minute install. It's inspired by oh-my-zsh — the idea being that Claude Code out of the box is like a bare terminal, and Forge gives it the full-featured setup developers actually want. It has 708 stars and was updated May 3, 2026.

## Why you'd want it (specific to your stack)
The agent roster (planner, architect, code-reviewer, tdd-guide, orchestrator) maps to workflows you're already doing manually. The `/orchestrate` command and the `team-orchestrator` skill would give your Jarvis multi-agent work a more structured scaffolding. The security hooks (6 layers) would be useful when Jarvis is running unsupervised during your focus blocks.

## Why I think it's worth your attention
It's the most comprehensive single-install Claude Code enhancement I've seen. But that comprehensiveness is also the concern.

## What I'd flag before you touch this
It bundles four external MCP servers: Playwright (browser automation), Jina Reader (web scraping), Chrome DevTools, and Context7. That's a significant external tool surface — these servers can make network calls and interact with your browser. Before using anything like this, I'd audit every MCP server's code separately. The risk is that a bundled MCP has permissions you didn't explicitly grant.

## What to do
React 👍 if you want me to deep-dive the source — I'll audit each MCP server and the hook files, flag any red flags, and give you a verdict on which components are safe to adopt (probably the skills and commands; probably NOT the bundled MCP servers without independent vetting).

🔗 https://github.com/sangrokjung/claude-forge

---
*Safety gate: third-party code with external MCP dependencies. Will not install. Deep-dive audit on request.*
