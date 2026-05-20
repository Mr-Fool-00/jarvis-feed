# Advisor Tool — BUILT

**Outcome:** Skill created at `~/.claude/skills/advisor-tool/SKILL.md`
**Date:** 2026-05-20

## What was built
A contextual skill that triggers when writing Claude API code with the advisor tool pattern. Covers:
- Exact beta header and tool parameters
- Valid model pairs (Haiku/Sonnet/Opus executor + Opus 4.7 advisor)
- Python and TypeScript code examples
- Cost control (max_uses, caching break-even at 3 calls, conversation-level budgeting)
- System prompt template for coding tasks (Anthropic's tested best practice)
- Multi-turn round-tripping rules
- Error handling and streaming behavior

## Why built (not skipped)
- Real Anthropic beta API with non-obvious parameters that need to be exact
- Caching break-even threshold (3 calls) is the kind of detail Claude would get wrong without the skill
- System prompt templates from Anthropic's internal testing are high-value reference
- Directly applicable to Leo's Jarvis Worker and any future API code
- Benchmarks show genuine cost savings: Sonnet+Opus advisor = 11.9% cheaper than Sonnet solo with quality lift
