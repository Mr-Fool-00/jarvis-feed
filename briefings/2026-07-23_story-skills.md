# Briefing: story-skills — Full Fiction Project Scaffold for Claude Code
**Date:** 2026-07-23 · **Score:** 8/10 · **Type:** B (third-party code — safety gate active)

## What it is

`danjdewhurst/story-skills` is a Claude Code plugin and skill package built specifically for long-form fiction. It ships a standardized project scaffold:

- `story-bible/` — characters, worldbuilding, factions, artifacts
- `plot-arcs/` — scene state per chapter, promises/payoffs tracker, continuity questions
- `timelines/` — in-world chronology across arcs
- `chapter-drafts/` — per-chapter output directory

Ships as both a Claude Code skill (`.claude/skills/`) and Codex plugin. Two standout components:

1. **Promises/Payoffs tracker** — every plot promise made in chapter N is logged; a continuity agent checks chapter N+1 onward for resolution. This creates an enforced contract between chapters.
2. **Continuity questions agent** — after each chapter draft, generates a list of unresolved threads for the next chapter's context window.

Source: https://github.com/danjdewhurst/story-skills

## Why it matters for Leo

This is the scaffold you've been rebuilding by hand on every project. The continuity tracking and promise/payoff system directly solves the chapter-drift problem — the same one you've been fighting manually. The architecture maps cleanly to what a Leo-native fiction pipeline needs.

## Safety gate

**DO NOT install or run any code from this repo without Leo's explicit review and approval.** This is third-party code from an unknown author. The concept is sound; the implementation needs to be read and understood before touching your pipeline.

## Recommended next step

Leo reviews the repo, extracts the promises/payoffs and continuity question patterns, and builds a native version for the Leo pipeline — using the architecture here as a reference, not a direct dependency.

## Build shape

- One skill: `fiction-continuity` — promises/payoffs tracker + per-chapter continuity questions
- Estimated effort: 2-3 hours to build a Leo-native version from scratch
- Risk of using third-party version directly: unknown author, no audit — medium risk
