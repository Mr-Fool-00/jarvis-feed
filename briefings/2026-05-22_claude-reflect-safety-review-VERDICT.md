# VERDICT: SKIPPED — claude-reflect

## Why

Existing tooling already covers the core value:

- **`/reflect`** (`~/.claude/commands/reflect.md`) — scans JSONL session logs for corrections, extracts abstract lessons, deduplicates against existing rules, presents a structured report, writes to CLAUDE.md with Leo's confirmation. This IS the native Stage 2 the briefing proposed building.
- **`/extract-lessons`** (`~/.claude/commands/extract-lessons.md`) — processes successful Max tasks for durable skills/memories. Covers the `/reflect-skills` pattern-discovery concept.

The only genuinely new piece from claude-reflect is **real-time hook-based auto-capture** (detect corrections as they happen and queue them). That's a `settings.json` hook configuration, not a skill or command — and the marginal value over scanning JSONL logs after the fact is low (you still need to review either way).

## What would change the call

If Leo finds `/reflect` too slow or wants corrections queued in real-time without running the command, a `UserPromptSubmit` hook that pattern-matches correction phrases and appends to a queue file would be a ~20-line addition to `settings.json`. Worth doing on request, not worth pre-building as a skill.

## Source assessment

- Repo: legitimate, MIT, 1k stars, active maintainer
- No security red flags found
- Architecture is sound (PostToolUse + Stop hooks, local-only)
- Just redundant with what we already have
