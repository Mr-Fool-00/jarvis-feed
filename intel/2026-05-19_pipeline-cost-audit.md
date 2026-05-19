# Pipeline Cost Audit — Refinements ON your existing 3-tier routing

**Date:** 2026-05-19 (Tuesday, ~10am CDT)
**Context:** Yesterday's ruflo study claimed "apply 3-tier model routing to cut your weekly Max budget 30-50%." Audit reveals: **your kindle pipeline already does 3-tier routing.** That claim is moot. This doc identifies the real gains still on the table.

## What you already do (confirmed by audit)

### Tier 1 — Pure Python ($0, runs locally)
Located at `~/Desktop/ai-writer/style-lab/scripts/`:
- `auto_pass2.py` — mechanical fixes (contractions)
- `mechanical_fixes.py` — mechanical fixes reporter
- `symmetry_check.py` — thematic symmetry detector (#1 AI structural tell)
- `quality_estimate.py` — blind score predictor from voice markers
- `compare_to_ledger.py` — 9.0 benchmark comparison
- `sentence_analysis.py` — sentence distribution (S9 marathon-sentence tell)
- `preflight.py` — preflight checks
- `scene_type.py` — scene type classifier (combat vs traversal)

Plus per-project scripts under each project's own `scripts/` dir.

### Tier 2 — Haiku ($0.0002/call equiv)
Per `war-without-end/.claude/agents/fixer-*.md` frontmatter:
- `fixer-01` (R19 forbidden words) — Haiku ✓
- `fixer-02` (R6 fragments) — Haiku ✓
- `fixer-03` (R1 sentence length + under-7-word density) — Haiku ✓
- `fixer-04` (R7 + R11 staccato) — Haiku ✓
- `fixer-05` (R3 similes) — Haiku ✓
- `fixer-06` (R4 metaphors/personification) — Haiku ✓
- `fixer-07` (R5 negation juxtaposition) — Haiku ✓
- `fixer-08` (R8/R9/R15) — Haiku ✓
- `fixer-09` (R10 + over-description) — Haiku ✓
- `fixer-10` (R12 filler) — Haiku ✓
- `fixer-11` (R2 em-dashes + semicolons) — Haiku ✓
- `fixer-12` (R13 + R14 possessives + cross-chapter recap) — Haiku ✓
- `fixer-13` (R16 + R17 teleporting + System format) — Haiku ✓
- `fixer-14` (sentence purpose audit) — Haiku ✓
- `fixer-polish-lean` — Haiku ✓

### Tier 3 — Sonnet (~$0.003-0.015/call equiv)
- `fixer-logic` (implied logic violations, omniscience leaks) — Sonnet ✓
- Chapter writing itself, pipeline orchestrator, verify_15 — probably Sonnet
- Judge panel — Sonnet

**Verdict:** You're already doing the architectural pattern ruflo claims as their breakthrough. The 30-50% gain they advertise IS your baseline, not your upside.

---

## The actual gains still on the table (4 specific moves)

### 1. Skip-if-empty optimization (estimated 15-25% Max-budget reduction)

**The hypothesis:** Many fixer dispatches result in PHASE 1 finding zero violations — and then PHASE 2/3 still run anyway, burning Haiku calls on no-op work.

**Audit needed:** Look at recent `checks/ch[N]_fixer_*_log.md` files. Count how many fixers found 0 violations but still ran. That's pure waste.

**The fix:** Add to pipeline-orchestrator: before dispatching fixer-N, run its PHASE 1 scan as a **pure Python script** (regex-based). If 0 hits → skip the Haiku dispatch entirely. If hits exist → dispatch with the pre-computed scan results so Haiku doesn't redo Phase 1.

Concretely, for each fixer-XX.md, derive a `phase1_scan_XX.py` that does the regex/lexical scan without LLM. Pre-existing scripts may already cover some (e.g. forbidden-words list for R19 is fully regex-derivable). Audit + extract.

**Estimated savings:** If 6 out of 15 fixers no-op on average chapter, that's 40% of fixer Haiku calls eliminated. Conservatively 15-25% weekly Max budget.

### 2. Cache reasoning across chapters (ruflo's ReasoningBank idea, ~10-15% Max-budget reduction)

**The hypothesis:** When fixer-08 explains "this sentence violates R9 because the brain-as-narrator pattern is X," that explanation is consistent across chapters. Re-deriving it every dispatch wastes tokens on the model "re-thinking" through R9's definition.

**The fix:** For each fixer, maintain a `cache/fixer-XX/canonical-violations.md` file with pre-derived violation patterns + canonical fixes. Inject this into the dispatch prompt as immutable reference, so the model just classifies new sentences against the cache rather than re-deriving the framework.

This is essentially codifying the fixer's "muscle memory" so it doesn't re-think Phase 1 each chapter.

**Estimated savings:** 10-15% on Haiku token volume.

### 3. Audit Sonnet usage for any drop-candidates (estimated 5-10% Max-budget reduction)

**Currently Sonnet-tier work (audit needed for actual list):**
- `fixer-logic` (implied logic + omniscience violations)
- Chapter writing
- Pipeline orchestrator
- Judge panel
- verify_15 adversarial scan

**Candidates to evaluate dropping to Haiku:**
- `verify_15` — adversarial re-scan. If the scan is pattern-matching against rules, Haiku may suffice. Sonnet only justified if catching subtleties Haiku misses.
- Pipeline orchestrator — if it's just routing + state management, Haiku is plenty. Sonnet is only needed for orchestrator-level reasoning about which step to retry/skip.
- Judge panel — depends. Judge needs nuance, so probably stays Sonnet.

**Audit method:** For each Sonnet-tier agent, sample 5 recent dispatches. If a Haiku rerun on the same input produces the same output, the agent can drop tier.

### 4. Cache "expensive" research at session start (ruflo's pre-warming pattern)

**The hypothesis:** Each chapter pipeline run re-reads character bibles, voice-DNA, world bibles, etc. Each read is a cache MISS that costs tokens.

**The fix:** At pipeline start, do a single bulk-read of canonical reference files into a `state/session_context.md` — the orchestrator passes THIS file to sub-agents instead of having each sub-agent re-read source files. One pre-warm vs N cache misses.

Trade-off: session-context.md gets stale if source files change mid-run. Mitigate with hash check.

**Estimated savings:** 5-10% on cache-read volume (the Anthropic usage panel field, NOT raw tokens). This is the cheapest category, so absolute savings are modest, but it's still free margin.

---

## Combined impact (if all 4 implemented)

- Skip-if-empty: 15-25% reduction
- Cache reasoning: 10-15%
- Sonnet→Haiku audit: 5-10%
- Pre-warm context: 5-10%

Stacked (not additive — overlap exists): realistic **30-45% reduction** in per-chapter Max-plan-budget consumption.

For Leo's stated baseline of "15-20% weekly budget per book" → drops to **9-14% weekly budget per book**.

Throughput implication: at 9-14%/book, **6-7 books/week becomes viable** at full budget, vs the current ~5 books/week ceiling. Over a 12-week summer, that's a meaningful 12-24 extra book-slots.

---

## Implementation sequence (post-finals)

**Week 1:**
- Implement skip-if-empty for the 5 most-frequently-empty fixers (audit logs to identify which)
- Measure delta on 3 chapters

**Week 2:**
- Implement cache-reasoning for the same 5 fixers
- Re-measure

**Week 3:**
- Audit Sonnet-tier agents, drop where safe
- Implement pre-warm session_context.md

**Week 4:**
- Full A/B: run 1 chapter on old pipeline, same chapter on new pipeline, compare cost + quality

If quality holds and cost drops as projected, ship to all projects.

---

## What this doesn't change

These optimizations don't affect:
- Quality of output (assuming Haiku/Sonnet drops only happen where they don't degrade)
- Pipeline structure or step count
- The orchestrator pattern itself

They're tuning, not redesign. Safe to layer onto existing system.

---

## Honest correction to yesterday's ruflo study

Yesterday's `2026-05-18_ruflo-study.md` said the 3-tier routing was the cherry-pick to take from ruflo. **That was wrong** — Leo already does 3-tier. The actually-transferable ideas from ruflo (now corrected):

- ❌ ~~3-tier routing — already done~~
- ✅ ReasoningBank-style caching — Idea 2 above
- ✅ Skip-if-empty / pre-warmed context — Ideas 1 + 4 above
- ✅ Self-learning loop (lightweight) — still relevant, separate doc

Updating the ruflo study with this correction is a low-priority cleanup.

---

*Audited 2026-05-19. Replaces yesterday's incorrect 3-tier-routing recommendation. Real gains are 30-45% via the 4 moves above. Post-finals work.*
