# Briefing: anthropics/claude-plugins-official — Official Anthropic Plugin Directory

**Date:** 2026-07-10 · **Score:** 8/10 · **Type:** A (Anthropic first-party)
**Source:** https://github.com/anthropics/claude-plugins-official
**Build verdict:** INFORMATIONAL — browse and selectively install first-party plugins. Safety gate still applies to anything community-contributed.

---

## What is this

Anthropic shipped a managed, official plugin directory: ~20 first-party Anthropic plugins plus vetted partner integrations, totaling 200+. This is the official answer to the unvetted third-party plugin ecosystem.

Install any plugin:
```
/plugin install <plugin-name>@claude-plugins-official
```

## First-party Anthropic plugins (the useful ones)

### Development workflow
- **dev-workflow** — Anthropic's curated development workflow conventions for CC sessions
- **mcp-server-dev** — Tools and patterns for building MCP servers inside CC
- **mcp-integration** — Skill for integrating external MCP servers into CC projects

### Design and content
- **frontend-design** — UI component patterns, responsive design conventions
- **skill-creator** — Patterns for creating and publishing CC skills from inside CC itself

### Language servers (11 plugins)
Language-specific plugins for Python, TypeScript, Rust, Go, Java, and others. These teach CC idiomatic patterns for each language — similar to what the Polars team shipped separately (PM digest #13) but for general language servers.

### Output styles
- **output-styles** — Configurable output formatting (structured vs narrative, verbosity levels)

## Why this matters

### 1. Vetted alternative to the wild west

Before this, installing a plugin meant going through unofficial repos or community aggregators. Anthropic managing a first-party registry with a vetting process changes the trust calculus significantly. The safety gate (never auto-install third-party code) still applies to community plugins within the registry, but the first-party 20 are Anthropic-authored.

### 2. skill-creator is the interesting one

A plugin designed specifically to help you create CC skills from inside CC — while using CC. This closes the loop: you can now build the tools that build the tools without leaving your session. This is directly relevant to the arXiv SOP paper from the PM digest (item #8: agents self-synthesize reusable procedures from recurring tool-call sequences).

### 3. mcp-server-dev

Anthropic explicitly built a plugin for building MCP servers inside CC. This is a signal about where they expect CC to grow — not just consuming MCP servers, but building them.

## What to do

1. Browse the first-party list at the GitHub link
2. Install any first-party plugins that match your current workflow
3. Do NOT install community plugins from this registry without reviewing their source first — the vetting process covers the first-party 20, not all 200+

## Note on community plugins in the registry

The registry includes vetted partner integrations (the ~180 non-first-party plugins). "Vetted" by Anthropic does not mean open-source reviewed — it means they passed Anthropic's partner intake. Still apply the same review standard you'd apply to any third-party tool before installing.

---

*Jarvis briefing · Run 2026-07-10 AM · jarvis-feed-agent*
