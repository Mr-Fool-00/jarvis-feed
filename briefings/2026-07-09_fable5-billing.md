# Briefing: Fable 5 Billing Begins July 8 — $10/M Input, $50/M Output; Promotional Window Extended to July 12

**Score:** 7/10 · **build_worthy:** ❌ NO (financial planning)
**Source:** releasebot.io, HN #48821102 · July 8, 2026

---

## What changed

Fable 5 transitioned from free promotional access to usage-credit billing on **July 8, 2026**:

| Plan | Rate |
|------|------|
| Claude Pro | $10/M input tokens, $50/M output tokens |
| Max | Usage credits (at plan tier rate) |
| API | $10/M input, $50/M output |

The promotional window was extended through **July 12** per Anthropic's HN announcement (#48821102). So: free until July 12, then credits apply.

---

## Cost math for Leo

At a 5:1 output-to-input ratio (typical for CC sessions with code generation):

| Model | Effective cost per 1M total tokens |
|-------|-------------------------------------|
| Fable 5 | ~$46/M effective |
| Opus 4.8 | ~$19/M effective |
| Sonnet 4.6 | ~$3/M effective |

**Fable 5 costs ~15× more than Sonnet 4.6 per token.** For work where Sonnet 4.6 is sufficient, using Fable 5 is pure spend. For work where quality matters (synthesis, complex reasoning, adversarial review), Fable 5 is often worth the premium.

The recommended model routing for a multi-agent pipeline:
- **Workhorse tasks** (file reading, code generation, test writing): Sonnet 4.6
- **Synthesis and judgment** (consolidating findings, ranking, final output): Opus 4.8
- **Highest-stakes decisions** (architecture review, security audit, adversarial check): Fable 5

---

## One note on the post-restoration Fable 5

Per item #9 in the digest, post-restoration Fable 5 is reportedly nerfed for C/Rust/memory-level work. If that affects your use case, Opus 4.8 may deliver better results than Fable 5 for systems code right now — and at $19/M vs $46/M effective, it's also significantly cheaper.

---

## Action

After July 12: watch your usage dashboard to see Fable 5 credit burn rate. Adjust model routing in any pipelines that defaulted to Fable 5 during the promotional period.
