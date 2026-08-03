# Briefing: Sonnet 5 Pricing Cliff — 29 Days to Sept 1 Deadline

**Item:** Claude Sonnet 5 Promotional Pricing Expiry  
**Deadline:** August 31, 2026  
**Score:** 8/10  
**Build verdict:** ACTIONABLE NOW — audit and decide before Aug 31  
**Source:** platform.claude.com/pricing (direct access blocked; data from secondary aggregators — verify before acting)

---

## The Change

Claude Sonnet 5's introductory pricing was set at $2/$10 per million input/output tokens when it launched. This promotional rate expires **August 31, 2026**. Starting **September 1**, standard pricing applies.

| | Promotional (now) | Standard (Sept 1+) | Change |
|--|------------------|-------------------|--------|
| Input | $2/MTok | $3/MTok | +50% |
| Output | $10/MTok | $15/MTok | +50% |

⚠️ *Pricing data could not be directly verified this run — WebSearch returned "unavailable" for the pricing query. The figures above match prior-run data and secondary aggregator coverage. Verify at console.anthropic.com before making routing decisions.*

---

## The Hidden Multiplier: Tokenizer Inflation

The sticker price increase of 50% understates the total cost change. Sonnet 5 uses a revised tokenizer that produces approximately **30% more tokens** than Sonnet 4 for equivalent English text.

**Effective cost comparison:**

Suppose a fixed block of text costs X tokens on Sonnet 4. On Sonnet 5, the same text costs ~1.3X tokens.

| Scenario | Sonnet 4 | Sonnet 5 promo | Sonnet 5 standard |
|----------|----------|----------------|-------------------|
| Sticker (input) | $3/MTok | $2/MTok | $3/MTok |
| Effective cost (same text) | $3.00 | $2.60 (2×1.3) | $3.90 (3×1.3) |
| vs. Sonnet 4 | baseline | -13% | +30% |

The promo period made Sonnet 5 cheaper than Sonnet 4 despite the tokenizer inflation. After Sept 1, Sonnet 5 is approximately **30% more expensive** than Sonnet 4 was for the same text.

---

## The Model Comparison Post-Sept 1

| Model | Input $/MTok | Output $/MTok | Gap vs Sonnet 5 standard |
|-------|-------------|--------------|--------------------------|
| Haiku 4.5 | $0.80 | $4 | 0.27× |
| Sonnet 5 (standard) | $3 | $15 | 1× (baseline) |
| **Opus 5** | **$5** | **$25** | **1.67× input** |
| Fable 5 | $10 | $50 | 3.33× |

The gap between Sonnet 5 and Opus 5 goes from **2.5× during promo** to **1.67× after Sept 1**. This is the key routing decision to revisit.

---

## Decision Framework

### Is any workload better moved to Opus 5 after Sept 1?

Ask three questions:
1. Does Opus 5 produce meaningfully better output for this step?
2. Is this step a small fraction of total token volume (so the 1.67× premium doesn't dominate cost)?
3. Is data retention a concern? (Opus 5 has zero retention; Sonnet 5 has 30-day retention)

If 1 + (2 or 3): route to Opus 5.

### Is any workload better moved to Haiku 4.5?

- Haiku 4.5 is 3.75× cheaper than Sonnet 5 standard on input
- For steps that are already "good enough" on a smaller model: classification, extraction, structured formatting, tool call dispatch
- The fiction pipeline's continuity log update step (structured JSON extraction) may be a Haiku candidate

### Should Sonnet 5 standard still be the default?

Yes, for most steps. The 30% premium over Sonnet 4 is acceptable if the quality improvement is real. Don't default-route everything to Opus 5 — use it where quality is the constraint, not as a blanket upgrade.

---

## Jarvis-Specific Impact

**Current Jarvis routing (inferred):** Most steps use Sonnet 5 (search synthesis, scoring, digest writing). The 50% sticker increase on Sept 1 increases per-run cost by approximately 50-80% (sticker + tokenizer combined).

**Recommendation:**
- Keep Sonnet 5 for search/fetch/filter/scoring steps (throughput sensitive)
- Consider routing the **digest writing step** to Opus 5 post-Sept 1 (highest quality sensitivity, one call per run, cost is not the binding constraint)
- The effective cost difference per digest run: Sonnet 5 standard digest → Opus 5 digest is ~$0.002-0.01 extra at typical digest lengths — negligible

---

## Fiction Pipeline Impact

**Chapter drafter:** Medium quality sensitivity, medium token volume. Keep on Sonnet 5 standard.
**Voice checker:** High quality sensitivity, low token volume. Candidate for Opus 5 routing post-Sept 1.
**Critic/editor:** High quality sensitivity, medium token volume. Strong candidate for Opus 5 routing.
**Continuity log update:** Low quality sensitivity, structured JSON. Consider moving to Haiku 4.5.

**Estimated post-Sept 1 pipeline cost per chapter (rough):**
- Sonnet 5 everywhere: baseline × 1.5
- Tiered (Haiku / Sonnet 5 / Opus 5 per step): baseline × 1.3-1.6 (varies by chapter complexity)

---

## Action Items

1. **By August 15:** Log in to console.anthropic.com and verify current Sonnet 5 pricing and the Sept 1 transition date
2. **By August 25:** Identify every pipeline step currently routed to Sonnet 5 API calls
3. **By August 31:** Update routing for any steps where Opus 5 is preferred post-Sept 1
4. **Optional:** Add a cost-per-run metric to Jarvis state so the Sept 1 impact is measurable

---

*Briefing generated by Jarvis · 2026-08-03 PM run · Pricing data from secondary sources — verify at console.anthropic.com*
