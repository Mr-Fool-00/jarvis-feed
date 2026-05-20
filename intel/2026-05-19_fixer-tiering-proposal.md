# Fixer Model-Tiering Proposal — 2026-05-19

## Honest assessment

**Action #3 in `2026-05-19_reel-patterns-actions.md` is partly stale.** It assumed the 14 fixers run on Sonnet by default and proposed moving mechanical ones down to Haiku. The actual current state (per `~/Desktop/ai-writer/kindle/CLAUDE.md` "Model Tiers" table + every fixer-NN.md frontmatter checked in `kindle/projects/war-without-end/.claude/agents/`) is the opposite: **fixers 01–14 and verify-15 are ALREADY on Haiku.** Only `fixer-logic` runs on Sonnet (judgment work, conditional dispatch). So the Sonnet→Haiku savings Action #3 estimated do not exist — they were already captured. The genuine remaining opportunities are smaller and run in two directions: (a) move a few quality-critical Haiku fixers UP to Sonnet 4.6 where prose judgment is being silently degraded (fixer-04 holistic choppiness, fixer-09 over-description, fixer-14 sentence-purpose), and (b) move the most mechanical Haiku fixers further DOWN to Haiku 4.5 explicitly (currently `model: haiku` is ambiguous — could resolve to 3.5 or 4.5 depending on harness). Honest token-savings ceiling: **~5-10% per chapter at best**, and probably zero if quality regressions force re-runs. The bigger win is naming the model version explicitly (4.5 vs 3.5) so behavior stops drifting between Claude Code releases. Recommendation: don't chase savings here — chase **stability** and **quality-up on the three judgment fixers**.

---

## Current state + proposed table

| Fixer | Owned rules / job | Current model | Proposed model | Rationale | Quality risk |
|-------|-------------------|---------------|----------------|-----------|--------------|
| fixer-01 | R19 forbidden AI words (find/replace from list) | haiku | **haiku 4.5 (pin)** | Pure list lookup. Could literally be regex. Pin to 4.5 explicitly. | Low |
| fixer-02 | R6 fragment (subject+verb completion) | haiku | **haiku 4.5 (pin)** | Sentence-local mechanical fix. | Low |
| fixer-03 | R1 over-50-words split + under-7 connect + R11 run-on split | haiku | **haiku 4.5 (pin)** | Length-bound mechanical splits. | Low-med (clause logic preservation matters) |
| fixer-04 | R7 staccato cluster + R11 **holistic passage choppiness** | haiku | **sonnet 4.6** | Holistic passage-level judgment ("sustained density across micro-paragraphs") is exactly the work Haiku underperforms on. This is the strongest UP-candidate. | High if left on Haiku — quality is silently degraded today. |
| fixer-05 | R3 simile → direct description | haiku | **haiku 4.5 (pin)** | Find pattern "like X" / "as X as" → rewrite. Mostly mechanical. | Low-med |
| fixer-06 | R4 metaphor + personification → direct | haiku | **haiku 4.5 (pin)** | Same shape as fixer-05. | Low-med |
| fixer-07 | R5 negation juxtaposition ("not X but Y") | haiku | **haiku 4.5 (pin)** | Pattern-match with combat-scene exemption. | Low |
| fixer-08 | R8 second-person + R9 because-omniscience + R15 brain-as-narrator | haiku | **haiku 4.5 (pin)** | Specific phrase patterns. POV reasoning is mostly local. | Med (POV judgment can be subtle) |
| fixer-09 | R10 over-description + mundane-over-description heuristic | haiku | **sonnet 4.6** | "Cut to the essential per scene's purpose" requires scene-purpose judgment — cannot be done cleanly at sentence level. Second-strongest UP-candidate. | High if left on Haiku — atmosphere stripping or under-cutting both happen at this tier. |
| fixer-10 | R12 roundabout + filler tightening | haiku | **haiku 4.5 (pin)** | Local sentence compression. | Low |
| fixer-11 | R2 em-dash cap (2/ch) + semicolon removal | haiku | **haiku 4.5 (pin)** | Count + replace. Maximally mechanical. | Low |
| fixer-12 | R13 possessive→definite + R14 cross-chapter recap flag | haiku | **haiku 4.5 (pin)** | Lookup + flag. | Low |
| fixer-13 | R16 teleporting bridge + R17 System notification formatting | haiku | **haiku 4.5 (pin)** | Insert bridge sentence / reformat block. Local. | Low-med |
| fixer-14 | Sentence-purpose audit (advances plot / reveals character / builds world / creates tension) | haiku | **sonnet 4.6** | Every sentence judged against four narrative purposes — pure judgment work, chapter-wide context required. Third-strongest UP-candidate. Currently the highest-risk Haiku assignment in the chain. | High if left on Haiku — wrong cuts or rubber-stamp passes both happen. |
| fixer-logic | Implied-logic family (R-IL: definite article on first contact, planted sensory behavior, etc.) | sonnet | **sonnet 4.6 (pin)** | Already correctly tiered. Pin version explicitly. | Low (already correct) |
| verify-15 | Adversarial re-scan + routing | haiku | **haiku 4.5 (pin)** | Mechanical 23-rule scan + comparison against pre-scan. No prose generation. | Low |

### Tier summary
- **Move UP (Haiku → Sonnet 4.6):** 3 fixers — fixer-04, fixer-09, fixer-14
- **Pin DOWN/SAME (haiku → haiku 4.5 explicit):** 11 fixers — 01, 02, 03, 05, 06, 07, 08, 10, 11, 12, 13, verify-15
- **Already correct (sonnet → sonnet 4.6 pin):** 1 fixer — fixer-logic

(11 + 3 + 1 = 15. Total fixers + verify = 15. Matches.)

### Net token effect (rough)
- 3 Haiku→Sonnet upgrades cost ~5x on those three steps' Sonnet portions only.
- 11 Haiku→Haiku-4.5 pins are cost-neutral (just version stability).
- **Expected per-chapter net: +3-8% tokens, not a savings.** But quality on the three judgment fixers stops being silently degraded — fewer Leo-edit overrides in human-sync (Step 18), which is where Leo's real time-cost lives.

If Leo wants a genuine cost cut, the lever is NOT model-tiering — it's **fewer fixer dispatches per chapter** (already partially captured by E7 verify-triad + section-surgeon path in `_core-sandbox`). That work is queued, not in this proposal.

---

## Implementation steps (if Leo approves)

The fixers exist as Claude Code subagent files (NOT skills). One canonical project: `kindle/projects/war-without-end/`. The `_core/` and `_core-sandbox/` agents/fixers/ directories are the upstream specs; project `.claude/agents/` files are dispatch wrappers with the `model:` frontmatter.

**Authoritative edit locations:**

1. `/Users/leograu/Desktop/ai-writer/kindle/projects/war-without-end/.claude/agents/fixer-NN.md` — change `model: haiku` to `model: sonnet` (or `model: claude-haiku-4-5-20250101` / equivalent ID once Leo confirms exact tag).
2. `/Users/leograu/Desktop/ai-writer/kindle/CLAUDE.md` — update the "Model Tiers" table to move fixer-04, fixer-09, fixer-14 from the Haiku row to the Sonnet row.
3. Repeat steps 1–2 for the other two active projects' agent copies:
   - `/Users/leograu/Desktop/ai-writer/kindle/projects/body-to-immortality/.claude/agents/` (if/when re-created — currently only `skills/` dir exists; no fixer agents)
   - `/Users/leograu/Desktop/ai-writer/kindle/projects/the-spire/.claude/agents/` (same)
   - Note: as of audit only war-without-end has the full fixer-agent set. Others apparently use a different dispatch route. Verify before editing.
4. `/Users/leograu/Desktop/ai-writer/kindle/_core/AGENT_COMMONS.md` — document the per-fixer model assignment as a single source of truth so the per-project frontmatter stops drifting.
5. Optional but recommended: add a comment to each fixer-NN.md frontmatter like `# locked 2026-05-19, rationale: holistic judgment work` so future passes don't silently revert.

**Exact frontmatter change pattern (fixer-04 example):**

```
# before
---
name: fixer-04
description: ...
model: haiku
---

# after
---
name: fixer-04
description: ...
model: sonnet
# Promoted Haiku → Sonnet 2026-05-19. Holistic passage-level choppiness (R11 sustained) requires multi-paragraph judgment that Haiku silently underperforms. Reverse only if Sonnet token cost forces budget pressure AND verify-15 confirms no quality regression on a 3-chapter A/B.
---
```

**Validation before promoting changes:**
- Run one chapter end-to-end with the new tiering.
- Compare verify-15 routing-back counts (lower = fewer fixer-loop passes = quality up).
- Compare Leo's human-sync edit volume on that chapter vs the prior 3-chapter rolling average.
- If routing-back drops AND edit volume drops, lock the change. If either rises, revert that specific fixer.

---

## Fixers to KEEP on current tier even when "cheaper would work"

- **fixer-03 (R1 length splits):** Could go even cheaper than Haiku-4.5 in theory (it's measurable). KEEP on Haiku because the clause-logic preservation constraint is non-trivial when splitting >50-word sentences; further downgrade risks subject-verb-object inversion noted in its own spec.
- **fixer-05, fixer-06 (simile, metaphor):** Tempting to go lower because the trigger is pattern-based. KEEP on Haiku because the *replacement* — direct description without substituting one figurative form for another — needs voice-DNA awareness Haiku-4.5 just barely holds.
- **fixer-08 (POV violations):** Pattern-match identifies it, but the rewrite must preserve close-third interiority. KEEP on Haiku, do NOT cheap further.
- **fixer-13 (R16 teleporting bridge):** Inserting a bridge sentence is a generation task with scene-state awareness. KEEP on Haiku — would actually be a fourth UP-candidate if Sonnet budget allowed, but downstream impact is lower than 04/09/14.
- **fixer-logic:** Already Sonnet. Never downgrade — this is the agent that catches omniscience leaks, which is the rule Leo cares about most ("NO OMNISCIENCE" is hard-coded in his user_profile).

---

## Open question for Leo

The model IDs in the frontmatter today say `haiku` and `sonnet` — bare aliases. Two paths:
- **A.** Leave aliases as-is and trust Claude Code's harness to map them to current 4.5 / 4.6. Risk: future Claude Code release silently shifts the mapping and quality drifts again.
- **B.** Pin explicit version IDs (e.g. `claude-haiku-4-5-20251101` / equivalent) in every fixer frontmatter. Risk: lockout when those IDs are deprecated.

Recommendation: **B**, with a quarterly pin-refresh ritual. Stability > convenience for a production pipeline.
