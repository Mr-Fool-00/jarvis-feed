---
ulid: BUILD-MPEFMN63-2026-05-20_ADVISOR-TOOL
slack_channel: C0B4C3K5NET
slack_user: U0B4JLHT2ER
slack_ts: 1779279972.042779
slack_thread_ts: 
created_at: 2026-05-20T19:05:45.675Z
status: pending
history_messages: 0
---
AUTO-BUILD FROM 👍 REACTION — Leo confirmed interest in briefing `briefings/2026-05-20_advisor-tool.md` by reacting 👍 in Slack.

Per Leo's safety rule (JARVIS_PERSONA.md):
- Deep-dive the underlying concept FIRST. Read the briefing in full, read source repos/papers/blog posts cited, check maintainer signals if it's third-party code, identify red flags.
- NEVER install or copy third-party code. Build a NATIVE version inspired by the pattern.
- Self-test before declaring done.

## YOUR WORKFLOW

1. Read `/Users/leograu/Desktop/jarvis-feed/briefings/2026-05-20_advisor-tool.md` in full.
2. Deep-dive any URLs/repos/papers it references (WebSearch + WebFetch).
3. Re-grade the briefing post-deep-dive: is it STILL ≥7/10, genuinely buildable as a Claude Code skill/command, and free of medium-or-higher red flags?

   IF YES → BUILD:
   - Create native version at `~/.claude/commands/<slug>.md` (slash command) OR `~/.claude/skills/<slug>/SKILL.md` (full skill with structure)
   - Match existing command style — read 2-3 commands in `~/.claude/commands/` for reference (council.md, research.md, html-output.md are good templates)
   - Self-test: re-read the file you just wrote, verify YAML frontmatter parses + content is coherent

   IF NO → SKIP:
   - Write a verdict file at `~/Desktop/jarvis-feed/briefings/<date>_<slug>-VERDICT.md`
   - Explain why you skipped (specific red flags found, redundant with existing tooling, not actionable as a skill, etc.)

4. Either way, commit + push:
   - BUILT outcome: commit with `briefing: [BUILT] <name> — created at <path>` prefix → fires #ai-news with the outcome
   - SKIPPED outcome: commit with `briefing: [SKIPPED] <name> — <one-line reason>` prefix

## CONSTRAINTS

- Working dir: `cd ~/Desktop/jarvis-feed` before git operations.
- Author the commit as Mr-Fool-00 (Leo's PAT) so slack-router routes correctly.
- Budget: ~10-15 min, ~3-5% of weekly Max plan budget.
- If the briefing's underlying concept turns out to be a non-buildable thing (news, acquisition, paper analysis, informational changelog), SKIP — don't force-build something that doesn't deserve a skill slot.

Return a one-line summary to Leo when done. The commit will trigger the slack-router post automatically.
