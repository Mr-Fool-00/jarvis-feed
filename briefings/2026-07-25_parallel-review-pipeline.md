# 8-Reviewer Parallel Review Pattern for Fiction Chapters — 8/10

**Date:** 2026-07-25
**Source URL:** https://claude.com/blog/ai-code-migration
**Score:** 8/10
**Category:** Architecture pattern — BUILDABLE (adapt parallel reviewer pattern for fiction pipeline)

---

## What it is

Anthropic published an official methodology post on July 24 documenting how they helped Jarred Sumner migrate 1M lines of Zig to Rust in under 2 weeks, and Mike Krieger move 165K lines of Python to TypeScript over a weekend, both using Dynamic Workflows.

The architectural core is a **parallel reviewer pattern**: for each migration chunk, they spawn 8 specialized subagents concurrently, each owning one review dimension:

1. Type safety correctness
2. Test coverage parity
3. API surface preservation
4. Security posture
5. Performance characteristics
6. Documentation accuracy
7. Target language idiom compliance
8. Regression prevention

All 8 run simultaneously (not serially). A judge subagent synthesizes their verdicts. **Phase gates** block the migration from advancing until all 8 reviewers emit APPROVED. If any reviewer fails, the chunk goes back for targeted repair — only the failing dimension needs to be re-reviewed, not all 8. After each full phase completes, the team ran three adversarial review rounds specifically looking for failure modes the individual reviewers missed.

---

## Why this directly applies to your fiction pipeline

The migration review dimensions map one-for-one onto fiction chapter review dimensions. The architecture is identical — the only difference is what each reviewer is checking:

| Migration dimension | Fiction dimension |
|---|---|
| Type safety correctness | Voice consistency (is this narrator voice or does it drift?) |
| Test coverage parity | Character continuity (are all referenced characters present and correct?) |
| API surface preservation | Plot logic (do scene-level events follow from prior causes?) |
| Security posture | World-bible compliance (geography, magic rules, lore) |
| Performance characteristics | Pacing (beat density per chapter, scene transitions, momentum) |
| Documentation accuracy | Handoff fidelity (does the closing scene set up the next chapter correctly?) |
| Language idiom compliance | Dialogue authenticity (voice-appropriate word choice, subtext) |
| Regression prevention | Sensory grounding (does the scene have physical, not just abstract, texture?) |

**Right now:** your chapter quality gate runs fixers serially. Chapter comes out → fixer 1 checks voice → fixer 2 checks continuity → etc. Each fixer runs sequentially and the whole chain takes as long as all the fixers combined.

**With this pattern:** all 8 reviewers run concurrently on the same chapter output. Wall-clock time drops from (N fixers × time per fixer) to (time of slowest single fixer + judge overhead). That's roughly 8× faster quality review at the same cost.

---

## The phase gate + adversarial round pattern

The migration blog documents something beyond just parallelism: a two-layer quality enforcement structure.

**Phase gate:** no chapter advances to the next arc until all 8 reviewers emit APPROVED. A single failure means targeted repair + re-review of only the failing dimension, not a full re-run.

**Adversarial round:** after each arc completes (all chapters reviewed), a separate adversarial agent round runs that is specifically prompted to find failure modes the individual reviewers missed. This is different from the per-dimension reviewers — it's looking at inter-chapter patterns (does the arc as a whole cohere?), cross-dimension interactions (does the pacing change cause voice drift?), and structural issues the single-chapter reviewers can't see.

For the fiction pipeline: run the adversarial round after each arc (3–5 chapters), not after every chapter. That's the natural scope boundary — a single adversarial agent with the full arc in context.

---

## What to build

A Dynamic Workflow script: `/chapter-review.js`

```js
// sketch of the pattern (not final code)
const REVIEWERS = [
  { key: 'voice', prompt: 'Review this chapter for narrator voice consistency...' },
  { key: 'continuity', prompt: 'Review this chapter for character continuity...' },
  { key: 'plot', prompt: 'Review this chapter for plot logic...' },
  { key: 'world', prompt: 'Review this chapter for world-bible compliance...' },
  { key: 'pacing', prompt: 'Review this chapter for pacing and beat density...' },
  { key: 'handoff', prompt: 'Review this chapter for handoff fidelity to next chapter...' },
  { key: 'dialogue', prompt: 'Review this chapter for dialogue authenticity...' },
  { key: 'sensory', prompt: 'Review this chapter for sensory grounding...' },
];

// All 8 run concurrently
const reviews = await parallel(
  REVIEWERS.map(r => () => agent(r.prompt + chapterText, { schema: REVIEW_SCHEMA, label: `review:${r.key}` }))
);

// Phase gate: block on any failure
const failed = reviews.filter(r => r.verdict !== 'APPROVED');
if (failed.length > 0) {
  // targeted repair: only re-review failed dimensions
  const repairs = await parallel(failed.map(r => () => agent(repairPrompt(r), ...)));
}

// Adversarial round (after arc, not after chapter)
// ... runs once per arc with full arc in context
```

The judge synthesizes the 8 reviewer verdicts into a single APPROVED/NEEDS_REPAIR/REJECT with a structured list of specific changes needed.

---

## How to kick it off

React 🚀 to this briefing post in #improvements. Jarvis will:
1. Draft the full `/chapter-review.js` workflow script with all 8 reviewer prompts
2. Write the REVIEW_SCHEMA (structured output for each reviewer)
3. Write the judge aggregator prompt
4. Add an arc-level adversarial round agent
5. Wire it into the existing fiction pipeline as a pre-commit quality gate

All fiction dimensions will need tuning against actual chapter output — Jarvis will draft them from the world bible and style guides in your project files.

🔗 https://claude.com/blog/ai-code-migration
