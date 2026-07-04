# Self-Suggestions — 2026-07-04 PM Run

**Run:** 2026-07-04 PM · Items reviewed: 8 surfaced, 10+ filtered

---

## #149 — Context compaction mid-run: research results must be written to scratch file before synthesizing

This run hit context window limits mid-research, forcing a summarization break. All research results survived through the summary mechanism, but this is the second time the context limit has interrupted a run between the "fetch/score" phase and the "write" phase. **Suggestion:** At the end of the research phase (Step 4), before beginning Step 5 (score/filter), write a `state/pm_scratch.json` file containing the current run's candidate items with scores and notes. This acts as a crash recovery checkpoint. If context compaction occurs, the write phase can read this file and resume without re-fetching.

## #150 — AGENT_RUNBOOK.md still missing detached HEAD fix in Step 0 (18th+ instance)

The detached HEAD fix (`git fetch origin main && git checkout -B main origin/main`) is documented in agent_suggestions.md suggestions #21, #23, #26, #79, #97, #103, #108, #111, #120, #123, #124, #128, #132, #137, #140, #142, #143, and now #150. It has never been added to AGENT_RUNBOOK.md Step 0. Every run that begins in a CCR container without this fix risks committing to a detached HEAD, which then fails to push. **Suggest Leo adds this as Step 0.1 in AGENT_RUNBOOK.md.** The command: `git fetch origin main && git checkout -B main origin/main`. No conditional — run it every time, it's safe even if already on main.

## #151 — Evidence gate pattern from Osmani post: apply to Jarvis fiction pipeline scene acceptance

From item #1 (Addy Osmani "Agentic Code Review"): the evidence gate pattern — requiring an agent to produce a structured evidence artifact before its output is accepted — directly maps to the fiction pipeline. Suggest adding a scene-acceptance step to the fiction pipeline: before a chapter-writer agent's scene is stored, prompt it to produce a 100-word evidence artifact (what happened, what continuity facts were established, what was verified). Store as `<scene>_evidence.md`. This makes the Continuity Auditor's input machine-readable rather than requiring full prose search.
