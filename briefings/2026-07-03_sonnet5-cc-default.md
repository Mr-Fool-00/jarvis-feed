# Briefing: Claude Sonnet 5 is Now the Default Model in Claude Code

**Date:** 2026-07-03
**Score:** 9/10
**For:** Leo — your Claude Code sessions (including Jarvis overnight runs) changed this week
**Verdict:** BUILD-WORTHY — two specific numbers need your attention before your next overnight fiction run

---

## What changed

Anthropic updated Claude Code's default model to **Claude Sonnet 5**. If you open a new CC session right now and don't specify a model, you're talking to Sonnet 5. That happened automatically — no opt-in required, no setting to flip.

Two things changed alongside the model:

**1. Context window: 1 million tokens.**
A 100,000-word novel is roughly 130,000–140,000 tokens with Sonnet 5's new tokenizer. The entire novel fits in context with room left over for character notes, world state, and revision history. No chunking, no mid-novel context resets.

**2. New tokenizer: 30% more tokens for the same text.**
This is the one you need to act on. Sonnet 5 tokenizes the same text as about 30% more tokens than Sonnet 4.x did. Any token budget you set in your CLAUDE.md or fiction pipeline configs was calibrated against Sonnet 4.x behavior. Under Sonnet 5, those same prompts and inputs now consume 30% more of your budget than expected.

---

## What this means for your overnight fiction runs

If Jarvis or your fiction pipeline has a line like "write a chapter of up to 3,000 tokens" — under Sonnet 5, that's actually consuming the equivalent of about 3,900 Sonnet 4.x tokens. Across a 10-chapter overnight run, you'd hit your budget wall earlier than expected.

**Practical check:**
- Open your CLAUDE.md and any fiction pipeline configs
- Find anywhere you've set a token limit, context budget, or chapter-length instruction
- Multiply those numbers by 0.77 to get the Sonnet 4.x-equivalent (or multiply your original Sonnet 4.x numbers by 1.30 to get the Sonnet 5 equivalent)

Or, simplest fix: bump any chapter-length token caps by 30% and let the 1M context absorb the overhead. Given the full context window, the main ceiling you'll hit is the 5-hour rate limit (which was just doubled — see the rate limit item in today's digest).

---

## Pricing (for your direct API calls, not Max plan sessions)

- **Through August 31, 2026:** $2 input / $10 output per million tokens
- **After August 31:** $3 input / $15 output per million tokens

Your Max plan covers interactive CC sessions. If Jarvis makes direct API calls (not through CC), those are billed at these rates.

---

## The bottom line

Sonnet 5 as default is a straightforward upgrade — more capable model, much larger context. The only thing that needs a quick adjustment is your token budget numbers: add 30% to account for the new tokenizer, and you're good.

🔗 https://www.anthropic.com/changelog/claude-sonnet-5-default-cc
