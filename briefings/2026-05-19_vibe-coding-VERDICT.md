# Verdict: SKIP — vibe-coding command not built

**Date:** 2026-05-19
**Briefing:** `2026-05-19_vibe-coding-agentic-engineering.md`
**Decision:** Do not build `/vibe-coding`. Use existing `/feature-dev:feature-dev`.
**Re-reviewed:** 2026-05-20 — verdict stands. Philosophy paper, covered by /feature-dev.

---

## What the briefing actually proposes

Simon Willison's essay is a **philosophy paper**, not a workflow. It argues that as Claude Code matures, "agentic engineering" (direct intent + verify outcomes, skip line-by-line review) becomes defensible. The briefing's literal "what to do" is: **read the essay.** There is no encoded process to turn into a slash command.

## Why feature-dev already covers it

The feature-dev plugin (`/feature-dev:feature-dev`) at `/Users/leograu/.claude/plugins/cache/claude-plugins-official/feature-dev/ae21a9367949/commands/feature-dev.md` is the operational instantiation of exactly what Simon describes:

| Simon's principle | feature-dev phase |
|---|---|
| Direct the work, don't review every line | Phase 1 (Discovery) + Phase 3 (Clarifying Questions) — Leo sets intent, asks clarifying questions, makes architecture decisions |
| Verify **outcomes** not **lines** | Phase 6 (Quality Review) — 3 parallel code-reviewer agents check correctness, conventions, DRY/elegance |
| Agent handles intermediate work | Phase 2 (code-explorer agents) + Phase 4 (code-architect agents) + Phase 5 (implementation) |
| Still review behavior + outcomes | Phase 6 reviewer agents + Phase 7 summary |

The subagents (code-explorer, code-architect, code-reviewer) are the **verification layer** Simon argues for. They run in parallel, return findings, surface high-severity issues — that is the outcome-verification net.

## What a `/vibe-coding` command would actually look like

To not overlap with feature-dev, it would have to be **less rigorous** — skip clarifying questions, skip architecture comparison, skip code-reviewer agents. That is just "regular Claude Code with no scaffolding." It doesn't deserve a command. It's the default.

The other direction — making it **more** rigorous — would duplicate feature-dev's 7 phases with marginal renaming. Cruft.

## The actionable takeaway from the briefing

Not a command. A behavioral note: **when Leo uses feature-dev this summer, trust the Phase 6 reviewer pass and don't second-guess intermediate diffs unless the reviewers flag something.** That is the operating-mode shift Simon's essay endorses, and feature-dev already implements it.

## Filed

- No new command at `/Users/leograu/.claude/commands/vibe-coding.md`
- Briefing acknowledged, no automation added
- Recommended usage: `/feature-dev:feature-dev <feature description>` for any non-trivial feature work this summer
