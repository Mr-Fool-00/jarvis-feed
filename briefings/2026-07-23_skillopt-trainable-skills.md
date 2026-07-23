# Briefing: SkillOpt — Skills as Trainable External State
**Date:** 2026-07-23 · **Score:** 8/10 · **Type:** A (research technique — no third-party code)

## What it is

arXiv 2605.23904 (Microsoft Research, May 2026). SkillOpt treats skill documentation as "trainable external state" — meaning the SKILL.md itself is updated based on execution feedback, not just the underlying model weights.

The loop:
1. Agent executes a skill (e.g., chapter-draft-skill)
2. Outcome is evaluated against a rubric (quality gate)
3. A skill-updater agent rewrites the SKILL.md based on what worked vs. what didn't
4. Next execution uses the improved skill doc

**Key numbers:** +20 point accuracy gain on multimodal benchmarks (0.73 → 0.93). Skills updated this way transfer across Claude Code and Codex without model retraining.

Source: https://arxiv.org/pdf/2605.23904

## Why it matters for Jarvis

Jarvis's skills are currently static — once written, they don't improve from real execution. Every run of the chapter-generation skill produces outcomes, but none of that feedback flows back into the skill doc. SkillOpt closes that loop.

The component you'd build is simple: an "eval + update" wrapper that sits outside any existing skill. It doesn't change the skill itself — it just adds:
1. An eval harness (pass/fail or rubric scoring)
2. A skill-updater agent that reads the eval output + current SKILL.md and produces an improved version

Run your chapter-generation skill through 10 books with this loop active and you'd have a materially better pipeline by book 11.

## Build shape

```
skills/
  chapter-draft/        # your existing skill (unchanged)
eval/
  chapter-eval.md       # rubric: voice consistency, pacing, continuity
  skill-updater.md      # agent: reads eval + skill, writes improved skill
scripts/
  run-eval-loop.sh      # wrapper: draft → eval → update → commit
```

- Estimated effort: 4-6 hours to build the eval harness + updater agent
- Risk: low — this is additive; it doesn't touch existing skills unless the updater runs
- Prerequisite: a working chapter-draft skill to improve against
