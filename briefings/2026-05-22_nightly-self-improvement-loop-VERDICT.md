# BUILT: Nightly Self-Improvement Loop → /reflect

## Outcome
Created `/reflect` slash command at `~/.claude/commands/reflect.md` (151 lines).

## What it does
Scans Claude Code JSONL session logs for correction signals (explicit corrections, implicit overrides, inefficiency patterns), extracts general lessons, deduplicates against existing CLAUDE.md rules, and appends new rules with Leo's confirmation.

## Key design decisions
- **Never auto-writes** — report is automatic, writing requires Leo's A/B/C/D/E choice of destination
- **Abstracts lessons** — "check imports before renaming" not "don't rename auth-middleware.ts"
- **Configurable scope** — last N hours, specific project, or all logs
- **Caps at 15 lessons per run** to avoid CLAUDE.md bloat
- **Privacy-respecting** — extracts patterns, never dumps conversation content

## Differs from extract-lessons
- extract-lessons: processes Max's task queue (structured, post-hoc)
- reflect: scans raw JSONL session logs across ALL Claude Code projects (real-time correction mining)

## Source inspiration
TDS article (May 2026), Addy Osmani's write-up, claude-reflect gist (1K stars). Native implementation — no third-party code.
