# Briefing: Fiction Voice Skill — Voice DNA Capture for Long-Form Projects
**Date:** 2026-07-23 · **Score:** 8/10 · **Type:** A (technique/guide — no third-party code)

## What it is

A step-by-step guide by Nicolas Cole (active fiction writer) on building a Claude Code skill that captures and maintains writing voice across long projects.

Core concept: extract stylistic patterns from existing prose → encode as a "voice DNA" skill → feed that skill into every chapter generation pass. The skill tracks:

- Sentence rhythm and length patterns
- Vocabulary range and register
- First-person narration style markers
- Emotional temperature per scene type
- Recurring structural tics (how you open chapters, how you close them)

Source: https://nicolascolefiction.substack.com/p/how-to-build-a-claude-cowork-skill

## Why it matters for Leo

Voice drift is one of the most persistent problems in AI-assisted long fiction. Chapter 1 and Chapter 12 start sounding different in subtle ways — different word choices, different rhythm. A voice DNA skill makes Leo's style an explicit, inspectable artifact that Claude holds in context through every chapter.

This is one of the clearest gaps in the current pipeline. There's no skill today that encodes Leo's voice.

## Build shape

**Step 1: Voice extraction** (one-time setup)
- Feed 3-5 sample chapters to a voice-extractor agent
- Agent produces a structured voice profile: sentence patterns, lexical preferences, rhythm markers, POV cues
- Output: `skills/voice-dna/voice-profile.md`

**Step 2: Voice skill** (used on every generation pass)
- A lazy-loaded skill that injects the voice profile into the chapter-draft context
- YAML frontmatter: `context: inject` (loads on demand, low token cost)
- Skill doc: `skills/voice-dna/SKILL.md`

**Step 3: Voice consistency check** (optional, pairs with SkillOpt)
- A post-draft eval that scores chapter output against the voice profile
- Flags drift before the chapter is committed

**Estimated effort:** 2-3 hours for extraction + skill authoring. The extraction agent is the key piece — the skill doc itself is just a structured output of that process.
