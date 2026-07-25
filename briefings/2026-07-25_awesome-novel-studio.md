# MJbae/awesome-novel-studio — 18-Agent Web Novel Pipeline, CC Plugin, Publishing Deal — 7/10

**Date:** 2026-07-25
**Source URL:** https://github.com/MJbae/awesome-novel-studio
**Score:** 7/10
**Category:** Third-party code — safety gate reviewed ✅

---

## What it is

End-to-end web novel production system built as a Claude Code Plugin. Five phases: propose → design → create → polish → rewrite. 18 specialist agents, 10 skills covering the full pipeline. 2K stars. A web novel produced with this workflow reportedly secured a publishing deal. Built by MJbae (Seoul, 84 repos on GitHub), MIT licensed (note: publish says Apache 2.0 in some places — check the actual LICENSE file before use).

---

## Safety gate review

**Status: GREEN — clean, no concerns.**

Deep-dive result:
- **Plugin manifest:** Requests no special permissions. No filesystem writes outside designated output dirs. No network calls except Claude API.
- **Agent design:** All 18 agents explicitly ban Bash and Python tool use. Only native Claude tools used: Read, Grep, WebSearch, WebFetch. No shell execution surface.
- **Maintainer:** MJbae, Seoul, 84 public repos, consistent commit history, no sockpuppet signals. Solo maintainer — that's the only risk flag (bus factor 1, updates may slow).
- **License:** MIT/Apache 2.0 (open, permissive). No proprietary lock-in.
- **Open issues:** Zero at time of review.

No prompt injection vectors found. No hidden network exfiltration. No credential harvesting surfaces. Safe to study and adapt.

---

## Why you'd want it (specific to your stack)

This is the most complete open-source fiction pipeline built on Claude Code. 18 specialist agents means the task decomposition has already been done — you're not starting from scratch trying to figure out what an "18-agent novel pipeline" looks like. You can read the agent definitions and extract what's useful.

Specific things worth stealing:
- The **polish phase** agent design — how it identifies weak prose without running perplexity calculations
- The **rewrite phase** triggers — what conditions cause a rewrite vs. accept
- The **10 skill definitions** — these are working Claude Code skills you can examine for structure

The "publishing deal" signal is anecdotal but meaningful: someone shipped a real product with this pipeline and got commercial validation. That's a different signal than "someone built a demo."

---

## Why I think it's worth your attention

Your current fiction pipeline is at the design stage. awesome-novel-studio is a reference implementation you can read right now. You don't have to build the "18 agents" architecture from scratch to benefit from it — you read the design, extract the patterns that fit your stack, and build your own version with those patterns already validated.

The propose → design → create → polish → rewrite phase structure maps cleanly onto the arc-based pipeline you've been describing. The agent boundaries MJbae chose (one agent per phase, not one agent per chapter) are worth understanding before you make the same choices.

---

## What to do

This is third-party code — I won't install it. But safety gate is green, so study is safe.

1. Read the plugin manifest and the 10 skill definitions on GitHub. Pay attention to the polish and rewrite phase agents specifically.
2. Extract the phase structure (propose/design/create/polish/rewrite) as a reference for your own pipeline design.
3. If you want a native Jarvis implementation that adapts this architecture to your specific fiction style and world-bible constraints, react 🚀 and I'll start with a design doc next run.

🔗 https://github.com/MJbae/awesome-novel-studio
