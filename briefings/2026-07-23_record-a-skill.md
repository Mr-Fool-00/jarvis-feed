# Briefing: Anthropic "Record a Skill" — Generate Skills from Screen Recordings
**Date:** 2026-07-23 · **Score:** 9/10 · **Type:** A (official Anthropic feature — no safety gate)

## What it is

Anthropic shipped a new way to create Claude Code skills: record your screen doing a task, and Claude generates a structured SKILL.md from the recording. Available to Pro, Max, and Team users. The generated skills use the standard `.claude/skills/` directory structure with YAML frontmatter and load lazily like any hand-authored skill.

No writing required — you demonstrate, Claude codifies.

Source: https://dataconomy.com/2026/07/22/anthropic-teach-claude-screen-recording-feature/
Official: https://www.anthropic.com/news/record-a-skill

*Note: This item arrived in the run's final research pass and was added retroactively to this digest after initial scoring.*

## Why it matters for Leo

This is the fastest path from "I do this manually every time" to "Claude does this for me." Zero authoring friction — no need to reverse-engineer your own workflow into a skill doc.

For the book pipeline specifically:
- Record a chapter-generation session → get a chapter-draft skill scaffold
- Record a continuity-check pass → get a continuity-checker skill
- Record a voice-review pass → get a voice-consistency skill

The output still needs refinement (the generated SKILL.md is a first draft, not a finished skill), but it compresses the extraction step from hours to minutes.

## Recommended immediate action

Before building anything from scratch:
1. Open Claude Code
2. Record a full chapter-generation session (start to first draft)
3. Let Anthropic generate the skill scaffold
4. Review + refine the output
5. Save as `skills/chapter-draft/SKILL.md`

Then run SkillOpt (see 2026-07-23_skillopt-trainable-skills.md) on it after 3-5 uses.

## Build shape

This is a workflow change, not a code build:
- Time to first skill via screen recording: ~45 minutes (record + review + refine)
- Pairs directly with SkillOpt briefing: record → refine → auto-improve via execution feedback
- Highest-leverage action from this run — do this before building anything else
