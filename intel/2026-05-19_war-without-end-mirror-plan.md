# War Without End Mirror Plan — Body-to-Immortality + The-Spire

**Date:** 2026-05-19
**Reference (gold):** `/Users/leograu/Desktop/ai-writer/kindle/projects/war-without-end/`
**Targets:** `/Users/leograu/Desktop/ai-writer/cultivation/body-to-immortality/`, `/Users/leograu/Desktop/ai-writer/tower-climber/the-spire/`
**Directive (Leo):** "for everything, mirror what war without end has, thats th best thing so far"

---

## Architectural Gap (the headline)

War-without-end is NOT a self-contained project. It is a **thin project node inside the `kindle/` universal writing system** (`kindle/_core/` + `kindle/genres/` + `kindle/projects/<project>/`). The project's `.claude/` directory holds peer-tier agents with `model: haiku|sonnet|opus` frontmatter; full specs live upstream in `kindle/_core/agents/` and `kindle/_core/agents/fixers/`. Project CLAUDE.md is just identity + `@context/` + tracker status (60 lines vs body-to-imm's 255 lines).

Body-to-immortality and the-spire are **legacy free-standing projects**. They duplicate the entire system in `system/agents/`, `system/WRITING_RULES.md`, `system/ANTI_PATTERNS.md`, etc., and use a template-style `pipeline-orchestrator.md` instead of war-without-end's `kindle-pipeline.md` / `kindle-pipeline-lean.md` peer agents. Both pre-date the `kindle/_core/` consolidation (2026-04-22 promotion event).

**Mirror = migrate them under `kindle/projects/`.** Half-mirroring (copy some agents but leave them as standalone projects) will produce drift again within a month.

---

## What War-Without-End Has (full structure)

```
kindle/projects/war-without-end/
├── .claude/
│   ├── agents/             # 35 peer agents w/ model:haiku|sonnet frontmatter
│   │   ├── fixer-01.md .. fixer-14.md  (haiku)
│   │   ├── fixer-logic.md  (sonnet)
│   │   ├── verify-15.md    (haiku) — note: name collision warning below
│   │   ├── chapter-writer.md (sonnet)
│   │   ├── chapter-brief.md (sonnet)
│   │   ├── chapter-audit.md / chapter-audit-lean.md
│   │   ├── kindle-pipeline.md (sonnet, full quality)
│   │   ├── kindle-pipeline-lean.md (sonnet, 6-phase $1.40/ch target)
│   │   ├── fixer-polish-lean.md (haiku)
│   │   ├── tracker.md / tracker-lean.md
│   │   ├── reader-sim.md (haiku) / reader-evaluation.md (sonnet)
│   │   ├── learning-extractor.md (sonnet)
│   │   ├── brief-validator.md (sonnet) / brief-hostile-check.md (sonnet)
│   │   ├── super-editor.md (sonnet) / exemplar-router.md (sonnet)
│   │   ├── dispute-arbiter.md (sonnet)
│   │   └── outline-generator-lean.md (sonnet)
│   └── plans/              # empty
├── CLAUDE.md (60 lines — identity + tracker status)
├── chapters/   (174 files)
├── briefs/ (27)
├── checks/ (270 dir entries — per-fixer logs)
├── disputes/ (29)
├── outlines/ (119)
├── trackers/ (22)
├── logs/ (64)
├── context/ (14) — story-bible.json + voice-dna + character bibles
├── voice-dna/ (5)
├── calibration/
├── drafts/
├── Aethon/                  # world-bible folder
├── assets/
└── ANTI_PATTERNS.md         # project-specific overrides only
```

Upstream parent `kindle/_core/`:
```
kindle/_core/
├── SYSTEM_DESIGN.md, SYSTEM_MAP.md, AGENT_COMMONS.md
├── WRITING_RULES.md (43KB), ANTI_PATTERNS.md (107KB)
├── CONFLICT_RESOLUTION.md, KNOWLEDGE_PATHS.md
├── MODEL_ROUTING.md, PIPELINE_STATE_SCHEMA.md
├── CALIBRATION_GROUND_TRUTH.md
├── scan.py (70KB) — mechanical hard-gate
├── autofix.py (20KB)
├── phase1/  templates/  scripts/
└── agents/                  # 44 files
    ├── orchestrator.md, chapter-writer.md, chapter-brief.md
    ├── chapter-audit.md, super-editor.md, reader-check.md
    ├── consistency-character.md, consistency-world.md, consistency-system.md, consistency-memes.md
    ├── brief-validator.md, brief-hostile-check.md, anchor-gate.md
    ├── QC-checker.md, prose-quality-gate.md, hostile-reader.md
    ├── scene-puncher.md, scene-stitcher.md, draft-repair.md
    ├── exemplar-curator.md, exemplar-router.md, boys-energy-scraper.md
    ├── reader-sim.md, voice-drift-check.md, beat-freshness-check.md
    ├── thread-closure-audit.md, pacing-check.md, pacing.md
    ├── prose.md, dialogue.md, grammar.md
    ├── learning-extractor.md, dispute-arbiter.md, tracker.md
    ├── regression-checker.md, report-card.md, memory-init.md
    ├── outline.md, outline-chapter.md, human-sync.md
    └── fixers/
        ├── 01-forbidden-words.md, 02-fragment.md, 03-sentence-length.md
        ├── 04-staccato.md, 05-simile.md, 06-metaphor.md
        ├── 07-negation.md, 08-pov.md, 09-over-description.md
        ├── 10-roundabout.md, 11-emdash.md, 12-second-reference.md
        ├── 13-continuity.md, 14-sentence-purpose.md, 15-sonnet-tells.md
        ├── fixer-logic.md, verify-15.md
        ├── section-rewriter.md, word-expander.md
        ├── FIXER_PROTOCOL.md, SONNET_TELLS.md
```

**Model assignment ground truth** (from war-without-end peer-agent frontmatter):

| Tier | Agents |
|---|---|
| **Haiku** | fixer-01..14, verify-15, fixer-polish-lean, chapter-audit-lean, tracker-lean, reader-sim |
| **Sonnet** | chapter-writer, chapter-brief, brief-validator, brief-hostile-check, chapter-audit, exemplar-router, super-editor, dispute-arbiter, learning-extractor, reader-evaluation, fixer-logic, kindle-pipeline, kindle-pipeline-lean, outline-generator-lean |
| **Opus** | calibration-writer only (per kindle/CLAUDE.md) |

Note `dispute-arbiter` is Sonnet in war-without-end's .claude/ (the kindle/CLAUDE.md describes it as "Haiku by default — Sonnet on complex tier conflicts" — actual frontmatter says sonnet). Mirror the frontmatter, not the prose description.

---

## What Body-to-Immortality Has

```
cultivation/body-to-immortality/
├── .claude/
│   ├── pipeline_state.json (basic schema)
│   └── skills/              # 4 SKILL.md files: draft, outline, resume, write
├── CLAUDE.md (255 lines, dual-phase template, dated 2026-04-24)
├── system/
│   ├── agents/              # 34 standalone agents, NO model frontmatter
│   │   ├── consistency-character.md, consistency-powers.md, consistency-memes.md
│   │   ├── emdash-fixer.md, era-fixer.md, fragment-fixer.md
│   │   ├── inference-fixer.md, negation-fixer.md, padding-fixer.md
│   │   ├── pov-fixer.md, reference-fixer.md, rhythm-fixer.md
│   │   ├── simile-metaphor-fixer.md, utility-fixer.md
│   │   ├── pipeline-orchestrator.md (47KB template)
│   │   ├── chapter-brief.md, chapter-writer.md
│   │   ├── dialogue.md, prose.md, grammar.md, pacing.md
│   │   ├── dispute-arbiter.md, super-editor.md
│   │   ├── batch-continuity.md, anchor-gate.md, scene-stitcher.md
│   │   ├── calibration-writer.md, calibration-judge.md
│   │   ├── regression-checker.md, report-card.md
│   │   ├── rules-checkpoint.md, memory-init.md
│   │   ├── outline.md, outline-chapter.md
│   │   └── pipeline/        # local fixer/QC skill specs (capitalized names)
│   ├── AGENT_COMMONS.md, ANTI_PATTERNS.md (23KB), CONFLICT_RESOLUTION.md
│   ├── KNOWLEDGE_PATHS.md, PIPELINE_STATE_SCHEMA.md, SYSTEM_MAP.md
│   ├── WRITING_PIPELINE.md (34KB), WRITING_RULES.md (10KB)
├── context/  knowledge/  outlines/  briefs/  checks/  disputes/  chapters/  trackers/  templates/  memory/  checkpoints/
```

Project also relies on **global `~/.claude/skills/fixer-NN/SKILL.md`** (1-15 + logic + protocol + section-rewriter + word-expander). The recent fixer-15 add at `~/.claude/skills/fixer-15/SKILL.md` was wired into this project's pipeline-orchestrator via the global Skill path, NOT a project-local file.

**Agents body-to-imm has that war-without-end LACKS:**
- `era-fixer.md` — genre-specific (cultivation era markers)
- `consistency-powers.md` — genre-specific (qi/meridian/cultivation stage)
- `inference-fixer.md` — pre-fixer-logic ancestor
- `simile-metaphor-fixer.md` — pre-split-into-fixer-05/06
- `utility-fixer.md` — catch-all
- `padding-fixer.md`, `negation-fixer.md`, `pov-fixer.md`, etc. — all SUPERSEDED by the numbered fixer-01..14 chain in war-without-end

**Agents war-without-end has that body-to-imm LACKS:**
- The whole numbered fixer chain (01..14 + logic + verify-15 — though verify-15 exists globally as Skill, no project-local agent w/ Haiku frontmatter)
- `kindle-pipeline.md` / `kindle-pipeline-lean.md` orchestrators (current arch is `pipeline-orchestrator.md` template)
- `fixer-polish-lean.md`, `chapter-audit-lean.md`, `tracker-lean.md`
- `brief-hostile-check.md`, `brief-validator.md`
- `reader-sim.md`, `reader-evaluation.md`, `learning-extractor.md`, `exemplar-router.md`
- `outline-generator-lean.md`
- The whole `kindle/_core/` reference layer (WRITING_RULES, ANTI_PATTERNS, SYSTEM_DESIGN, scan.py, autofix.py)
- `15-sonnet-tells.md` / `fixer-15.md` peer agent with Haiku frontmatter (it only exists globally as Skill — see Blocker question)

---

## What The-Spire Has

Same general shape as body-to-imm but **MORE divergent + older**:

```
tower-climber/the-spire/
├── .claude/
│   ├── pipeline_state.json (empty {})
│   ├── skills/ (5 SKILL.md: apply-disputes, draft, outline, resume, write)
│   └── plans/
├── CLAUDE.md (264 lines)
├── system/
│   ├── agents/              # 33 agents, similar set to body-to-imm
│   │   ├── (same fixer set: emdash, era, fragment, inference, negation, padding, pov, reference, rhythm, simile-metaphor, utility)
│   │   ├── pipeline-orchestrator.md
│   │   ├── romance-editor.md  ← UNIQUE to the-spire
│   │   ├── state-updater.md   ← UNIQUE to the-spire
│   │   └── (no pipeline/ subdir for skill specs)
│   ├── (all the legacy layer files: LAYER_0..LAYER_12, PERSONALITY.md, REFINEMENT_PROTOCOL.md, MEMORY_SYSTEM.md, IMPORT_PROTOCOL.md, INTEGRATION_GAPS.md, DEFERRED_QUESTIONS.md, GENRE_VOCABULARY.md)
│   └── (no pipeline/ subdir of fixer skill specs)
├── (no .claude/skills/apply-disputes in body-to-imm — only in the-spire)
```

The "10 fixer slots vs 14" framing is misleading on closer look. The-spire has 11 distinct fixer agents (emdash, era, fragment, inference, negation, padding, pov, reference, rhythm, simile-metaphor, utility) — same family as body-to-imm. Neither project uses the war-without-end numbered chain. Both projects use the OLD named-fixer architecture. The "14 slots" in war-without-end is a reorganization, not an addition.

**Spire-unique:** `romance-editor.md`, `state-updater.md`, the older Phase 1 layer files (LAYER_*.md), `IMPORT_PROTOCOL.md`, `INTEGRATION_GAPS.md`, `DEFERRED_QUESTIONS.md`, `GENRE_VOCABULARY.md`, `REFINEMENT_PROTOCOL.md`, `MEMORY_SYSTEM.md`, `PERSONALITY.md`.

The-spire is essentially a frozen February-March 2026 snapshot. It has not been touched since March 23.

---

## Concrete Migration Table

| Component | War-Without-End | Body-to-Imm | The-Spire | Action: body-to-imm | Action: the-spire | Risk |
|---|---|---|---|---|---|---|
| **Project location** | `kindle/projects/<x>/` | `cultivation/<x>/` | `tower-climber/<x>/` | Move project under `kindle/projects/body-to-immortality/`. Update parent CLAUDE.md `Active Projects` table. Update global memory pickup file. | Move under `kindle/projects/the-spire/`. | HIGH — path refs in trackers, checks, briefs, logs. ~270 dir entries in checks/ each. Run grep for hardcoded paths first. |
| **Shared system files** | Reads `kindle/_core/WRITING_RULES.md`, `ANTI_PATTERNS.md`, `SYSTEM_DESIGN.md`, etc. | Own copies in `system/` | Own copies in `system/` | After move: delete project's `system/AGENT_COMMONS.md`, `WRITING_RULES.md`, `WRITING_PIPELINE.md`, `KNOWLEDGE_PATHS.md`, `CONFLICT_RESOLUTION.md`, `PIPELINE_STATE_SCHEMA.md`, `SYSTEM_MAP.md`. Keep project-local `ANTI_PATTERNS.md` ONLY if cultivation-specific (current 23KB likely is genre-flavored; review first). | Same. Delete LAYER_*.md files (kindle/_core/phase1/ replaces). Delete PERSONALITY.md, REFINEMENT_PROTOCOL.md, MEMORY_SYSTEM.md, IMPORT_PROTOCOL.md, etc. — these belong to `kindle/_core/phase1/`. | MED — anything referencing these in old chapters/briefs becomes a broken link. Project should be far enough along that historical artifacts are sealed. |
| **.claude/agents/** | 35 peer agents w/ frontmatter | Does not exist | Does not exist | Create `.claude/agents/` with frontmatter-bearing peer agents copied from war-without-end: fixer-01..14 (haiku), fixer-logic (sonnet), verify-15 (haiku), chapter-writer (sonnet), chapter-brief (sonnet), brief-validator (sonnet), brief-hostile-check (sonnet), chapter-audit (sonnet) + chapter-audit-lean (haiku), super-editor (sonnet), dispute-arbiter (sonnet), exemplar-router (sonnet), learning-extractor (sonnet), reader-sim (haiku), reader-evaluation (sonnet), tracker (haiku) + tracker-lean (haiku), kindle-pipeline.md (sonnet), kindle-pipeline-lean.md (sonnet), fixer-polish-lean (haiku), outline-generator-lean (sonnet). The peer agents are 30-50 line stubs; full specs live in `kindle/_core/agents/`. | Same. | LOW per file but volume = MED. ~35 files. All near-identical (point at `kindle/_core/agents/`). |
| **Numbered fixer chain (01..14 + logic + 15)** | Present as `.claude/agents/fixer-NN.md` peer agents, full specs in `kindle/_core/agents/fixers/NN-name.md` | Uses **global `~/.claude/skills/fixer-NN/SKILL.md`** (1-15 wired up); old `system/agents/<named>-fixer.md` files orphaned | Old named fixers, no numbered chain | Add `.claude/agents/fixer-NN.md` peer agents that point at `kindle/_core/agents/fixers/NN-name.md`. Per war-without-end's own pipeline orchestrator: **MUST dispatch fixers via `Task(subagent_type=fixer-NN)` not via Skill** — Skill dispatch inherits Sonnet context and defeats Haiku tier (ch27 $10.89 regression in war-without-end was traced to exactly this). The recent fixer-15 Skill wiring is OK for one-off `humanize` use but for in-pipeline use the project needs a peer agent. | Same. | HIGH — this is the model-tier bug that cost $10.89 on a single chapter. Mirroring fixer chain MUST include the peer-agent pattern, not just copying skill paths. |
| **Genre extensions** | `kindle/genres/military-fantasy/GENRE.md` adds consistency-genre, war-specific trackers | Has `consistency-cultivation.md` + `consistency-powers.md` standalone | Has no genre dir analog | Create `kindle/genres/cultivation/`. Move `consistency-cultivation.md` + `consistency-powers.md` + `consistency-character.md`-genre-bits there per `kindle/CLAUDE.md` genre extension pattern. Update orchestrator dispatch list to include genre-consistency agent at Step 9. | Create `kindle/genres/tower-climber/` (already promised in `kindle/CLAUDE.md` table: "consistency-tower, floor_tracker.json, ability_registry.md"). | MED — net new structure, but kindle/CLAUDE.md already documents the pattern. |
| **CLAUDE.md** | 60 lines: identity, `@context/`, tracker status, status JSON | 255 lines: full dual-phase template w/ inline agent table, model tiers, pipeline diagram | 264 lines: same template, Book A flavor | Replace body-to-imm CLAUDE.md with war-without-end style: identity + `@context/` + status. Move surviving project-specific behavior (phase detection, cultivation hard-no's) into context files. The dual-phase logic is now in `kindle/CLAUDE.md` + `kindle/_core/phase1/`. | Same. | LOW — fresh CLAUDE.md doesn't break the past. Validate phase-1-was-completed flag is preserved (story-bible.json existence). |
| **pipeline-orchestrator.md (template)** | Replaced by `kindle-pipeline.md` + `kindle-pipeline-lean.md` peer agents | 47KB template at `system/agents/pipeline-orchestrator.md` | Same | Delete after pipeline.md / pipeline-lean.md added to .claude/agents/ AND the current chapter has finished. The recent fixer-15 wiring was added to this file — that wiring needs to be re-added to the new kindle-pipeline-lean.md fixer chain section if not already present. | Same. | HIGH — this is the file mid-pipeline state machine. Don't delete mid-chapter. Wait for a clean "no pipeline in progress" boundary. |
| **.claude/skills/** | None (skills are global) | 4 local skills: draft, outline, resume, write (SKILL.md each) | 5 local skills: + apply-disputes | Decide: keep as project-local convenience commands, OR migrate to global. Recommendation: keep local until kindle/ documents a slash-command pattern. They're harmless. | Same. | LOW. |
| **state-updater.md (the-spire only)** | Step 21 logic lives in orchestrator + `tracker.md` | Inlined in orchestrator | Standalone agent | n/a | Delete after migration to kindle-pipeline.md; functionality is in tracker.md + Phase 21 of orchestrator. | LOW. |
| **romance-editor.md (the-spire only)** | n/a | n/a | Standalone | n/a | Delete — the-spire is "Zero romance, not a hint of potential" per CLAUDE.md. Dead agent. | LOW. |
| **Story-bible / voice-dna / character-bibles** | `context/` dir with story-bible.json, voice-dna files | `context/core/story-bible.json`, voice-dna | Same | Keep as-is, no migration needed. War-without-end uses identical convention. | Same. | LOW. |
| **Existing chapters/briefs/checks** | 174 chapters in `chapters/` | Unknown count | Few | Leave in place after move. Re-pathing handled by project move. | Same. | MED — any absolute path inside checks/ files will break. Grep first. |

---

## Destructive Migrations (call out)

1. **Project relocation** `cultivation/<x>/ → kindle/projects/<x>/` and `tower-climber/<x>/ → kindle/projects/<x>/`. Affects every absolute path stored in trackers, checks, briefs. Mitigation: grep for the old path string, mass-replace. Pre-move: git commit clean. Post-move: re-test `/status` from new location before doing anything else.

2. **Delete project-local `system/WRITING_RULES.md` + `ANTI_PATTERNS.md` (universal portions only)**. Once project reads from `kindle/_core/`. Mitigation: diff project's ANTI_PATTERNS.md against `kindle/_core/ANTI_PATTERNS.md` first; cherry-pick project-genre-specific patterns into a `kindle/genres/<genre>/ANTI_PATTERNS.md` overlay before delete.

3. **Delete the `pipeline-orchestrator.md` template** in each project. ONLY after the new `kindle-pipeline.md` is wired up AND no pipeline is mid-run. The recent fixer-15 wiring goes away with this file; the same wiring must be present in kindle-pipeline-lean.md's fixer chain before delete.

4. **Delete LAYER_*.md from the-spire's `system/`**. These are Phase-1-questionnaire artifacts now centralized in `kindle/_core/phase1/`. Risk: zero if the-spire's Phase 1 is complete (story-bible.json exists). Verify before delete.

5. **Drop the old named fixers** (`emdash-fixer.md`, `era-fixer.md`, `fragment-fixer.md`, `inference-fixer.md`, `negation-fixer.md`, `padding-fixer.md`, `pov-fixer.md`, `reference-fixer.md`, `rhythm-fixer.md`, `simile-metaphor-fixer.md`, `utility-fixer.md`) once numbered chain is wired and one test chapter passes end-to-end. Risk: low if everything else passes. These files are explicitly named in the orchestrator's dispatch sequence; that sequence is being replaced.

---

## Effort / Impact Ranking

| # | Action | Effort | Impact | Why |
|---|---|---|---|---|
| 1 | **Drop in `.claude/agents/` with peer-agent files** (35 files, ~50 lines each, point at kindle/_core/) for body-to-imm | S | **VERY HIGH** | Fixes the Haiku-tier dispatch bug. Without this, every fixer Task on body-to-imm runs Sonnet via Skill = same ch27 cost regression pattern. This is the single highest impact-to-effort change. |
| 2 | **Add `kindle-pipeline-lean.md` to body-to-imm `.claude/agents/`** | S | HIGH | Unlocks the $1.40/ch lean orchestrator. Currently body-to-imm uses the 47KB template orchestrator on Sonnet. |
| 3 | **Move body-to-imm under `kindle/projects/`** + strip duplicate system files | M | HIGH | Eliminates drift permanently. Project starts reading `kindle/_core/WRITING_RULES.md` (43KB, current) instead of its frozen 10KB copy. |
| 4 | Repeat 1–3 for the-spire | M | HIGH | The-spire is more divergent; needs LAYER_*.md cleanup. But it's also more dormant (untouched since March 23) so timing risk is low. |
| 5 | Create `kindle/genres/cultivation/` w/ consistency-cultivation + consistency-powers; update orchestrator dispatch | S | MED | Slot the genre-specific agents per the documented pattern. |
| 6 | Create `kindle/genres/tower-climber/` per the kindle/CLAUDE.md promised list | M | MED | Per docs it's promised but not built. Needs floor_tracker.json + ability_registry.md design. |
| 7 | Rewrite project CLAUDE.md to war-without-end short-form for body-to-imm + the-spire | S | LOW | Cosmetic but reduces context bloat at session start. |
| 8 | Delete legacy named fixers + romance-editor + state-updater after dust settles | S | LOW | Cleanup only. |

**Top 3 by impact-to-effort: 1, 2, 3 (all body-to-immortality). The-spire is dormant — defer until body-to-imm is verified working end-to-end on at least one chapter.**

---

## Fixer-15 Status

- **Global Skill exists:** `~/.claude/skills/fixer-15/SKILL.md` (just wired this session)
- **kindle/_core has it:** `kindle/_core/agents/fixers/15-sonnet-tells.md` exists (the full spec — referenced as "15-sonnet-tells" in kindle/CLAUDE.md model routing as conditional Haiku)
- **War-without-end peer agent:** NO `.claude/agents/fixer-15.md` exists in war-without-end. The kindle-pipeline.md does not reference fixer_15/FIXER_15. The kindle-pipeline-lean.md doesn't either.
- **Action needed for war-without-end:** Add `.claude/agents/fixer-15.md` peer agent (Haiku frontmatter) pointing at `kindle/_core/agents/fixers/15-sonnet-tells.md`. Then add a Step-6 dispatch call to it in both `kindle-pipeline.md` and `kindle-pipeline-lean.md` (after fixer-14, before verify-15). This brings war-without-end up to body-to-imm's recent improvement. YES, war-without-end needs the fixer-15 addition too — it's not yet present there.

## Naming-Collision Warning

War-without-end has `.claude/agents/verify-15.md` (Haiku, adversarial re-scan, NOT a fixer) AND the new fixer-15 is also "15"-themed. The "15" in verify-15 refers to "15th-and-final step of the fixer chain audit" — historical. The fixer-15 added this session is a separate concept ("final humanize pass that strips AI-tell residue"). When mirroring, name the new agent `fixer-15` (not `verify-15-something`) — there is precedent: the kindle/_core/agents/fixers/15-sonnet-tells.md is already the "15th fixer" naming slot. Just add it. The Skill name `fixer-15` matches.

---

## Incompatibilities With Mirroring

1. **Phase 1 dual-phase logic in CLAUDE.md.** Body-to-imm and the-spire have ~150 lines of Phase 1 vs Phase 2 detection logic. War-without-end is past Phase 1; its CLAUDE.md skips this entirely (it's deferred to `/begin` in `kindle/CLAUDE.md`). For body-to-imm (also past Phase 1 — story-bible.json exists) this is fine to strip. For the-spire, **confirm story-bible.json exists** before stripping.

2. **Project-local skills (`.claude/skills/draft|outline|write|resume|apply-disputes`).** War-without-end has zero local skills. Body-to-imm and the-spire have these as small SKILL.md files. They're harmless and project-convenience; not strictly an incompatibility but a divergence from "pure mirror." Recommend keeping for now until kindle/ documents the slash-command pattern formally.

3. **Cultivation-specific ANTI_PATTERNS.** Body-to-imm's `system/ANTI_PATTERNS.md` (23KB) likely contains cultivation-genre-specific patterns (qi/meridian phrasing pitfalls, cultivation-stage register, etc.) that don't belong in universal `kindle/_core/ANTI_PATTERNS.md` (107KB universal). These need genre overlay before any deletion.

4. **The-spire's older Phase 1 file format.** LAYER_0..LAYER_12_VOICE files. These are an older questionnaire format that predates kindle/_core/phase1/. The story-bible may have been built from these directly. If story-bible.json exists and is current, the LAYER files are archive-only — safe to move to `_archive/` rather than delete outright.

---

## Sequencing Recommendation

**Phase A — body-to-imm (active project, fix bug first):**
1. Add `.claude/agents/` with 35 peer-agent stubs (1–2 hours of mechanical work). Test by running `/write` on one chapter — confirm fixer dispatches log Haiku tier in cost ledger.
2. Verify chapter passes end-to-end with new fixer chain (verify-15 routes back to numbered fixers correctly).
3. ONLY THEN: move project under `kindle/projects/` and strip duplicate system files. Grep absolute paths first.
4. Repath updates in any tracker/log file that has hardcoded `cultivation/body-to-immortality/` strings.
5. Replace CLAUDE.md with short-form war-without-end-style.
6. Create `kindle/genres/cultivation/` and reroute consistency-cultivation / consistency-powers there.

**Phase B — war-without-end gap:**
7. Add `.claude/agents/fixer-15.md` to war-without-end. Wire it into kindle-pipeline-lean.md fixer chain (and kindle-pipeline.md if quality pipeline is still used). This is the cross-project consistency catch-up from Leo's recent global skill add.

**Phase C — the-spire (defer; dormant):**
8. Same as Phase A, but with LAYER_*.md → archive, romance-editor + state-updater delete.
9. Create `kindle/genres/tower-climber/` per documented promised list (floor_tracker.json, ability_registry.md, consistency-tower).

Don't start Phase C until Phase A is validated on a real chapter. The-spire being dormant means there's no cost to waiting and significant cost to migrating blind.

---

## Open Questions / Blocker for Leo

**ONE BLOCKER:** Migration scope ambition.

Pick one — this changes the whole plan:

**A) Full mirror.** Move both projects under `kindle/projects/`, strip duplicate system files, build genre extensions. ~6-10 hours total. Permanently eliminates drift. Requires Leo to accept that the migration is the actual project mid-flight.

**B) Surface mirror.** Leave both projects where they are. Just add `.claude/agents/` with the 35 peer-agent files in each. Fixes the Haiku-tier dispatch bug + brings in the lean pipeline. ~1-2 hours per project. Doesn't fix the duplicate-system-files drift problem, but unblocks the highest-impact issue immediately.

**C) Body-to-imm full mirror, the-spire surface-only.** Body-to-imm is active (recent fixer-15 wiring) so it's worth the full move. The-spire is dormant — defer the structural move until it reactivates. ~4-6 hours total now.

Recommend **C**. Fixes the active project properly, defers risk on the dormant one. Leo's recent fixer-15 work was on body-to-imm so the energy is already there.
