# wshobson/agents Study Notes — 2026-05-18

## What it is

`wshobson/agents` on GitHub — comprehensive Claude Code plugin system with **185 specialized agents** across **80 plugins** (153 skills, 100 commands).

## What the Reel was probably referencing

The "91k stars, 147 agents" repo from the Reel (DYZs9FiDIii) doesn't exactly match wshobson/agents (35.6k stars, 185 agents). Two possibilities:
1. The Reel inflated numbers (common in clickbait Reels)
2. There's another repo I haven't found

wshobson/agents is the most well-known repo in this space, well-maintained, and installable via plugin marketplace. It's the practical equivalent of what the Reel was describing.

## Install path (safe — plugin marketplace, no daemon)

```
/plugin marketplace add wshobson/agents
/plugin install python-development        # or any of 80 plugins
```

Same pattern as ruflo Path A: slash commands + agent definitions, NO files written to workspace, NO MCP server registered. Easy to install per-plugin and easy to uninstall.

## What's worth trying

Look at the plugin list (would need to browse `github.com/wshobson/agents` plugin directory) for ones that map to Leo's actual needs:

Likely candidates worth a look:
- A "writing/editing" plugin if one exists — would complement his fixer chain
- An "AI/agent development" plugin — useful for Jarvis build work
- Specialty agents like "Tax Strategist," "AI Citation Strategist" (mentioned in the Reel) — niche but interesting

**Don't install all 80 plugins.** Pick 2-3 based on actual current need. Avoid context bloat.

## Recommendation

**Post-finals, in a sandbox repo:**
1. `/plugin marketplace add wshobson/agents`
2. Browse `github.com/wshobson/agents` for 2-3 plugins matching current needs
3. `/plugin install <one>` and test it for an hour
4. If useful, keep; if not, `/plugin uninstall` and try another

Specifically check for "AI Citation Strategist" (the Reel claimed it optimizes your brand to appear in ChatGPT/Claude/Gemini/Perplexity answers — could be relevant to revenue-path E content marketing).

## What I won't do tonight

Install in his real environment. Plugin marketplace adds slash commands globally — if any of the 100 commands conflict with his existing `/council`, `/morning-brief`, `/humanize`, `/prompt-ladder`, that's a problem. Better to test in a sandbox first.

---

*Studied 2026-05-18. Recommendation: try plugin-by-plugin in sandbox post-finals.*
