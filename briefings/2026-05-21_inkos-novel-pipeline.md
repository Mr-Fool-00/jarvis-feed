# InkOS: 10-Agent Novel-Writing Pipeline — 9/10

## What it is
An open-source CLI tool (Narcooo/inkos on GitHub) that writes, audits, and revises entire novels autonomously through a chain of 10 specialized agents. Each agent handles one step — planning, context retrieval, structure, prose generation, observation, state extraction, normalization, auditing against 33 dimensions, and revision. The system maintains 7 canonical "truth files" as its persistent world-state so agents don't drift or contradict earlier chapters.

## Why you'd want it (specific to your stack)
Your writing pipeline already runs 14 fixers + verify-15 + collective memory. InkOS solves the exact gap you haven't closed yet: **persistent world-state tracking across chapters**. Right now your fixers re-derive character locations, relationships, and resource states from the chapter text each pass. InkOS's truth files (current_state.md, particle_ledger.md, pending_hooks.md, subplot_board.md, emotional_arcs.md, character_matrix.md, chapter_summaries.md) are a canonical ledger that every agent reads before acting and updates after acting — Zod-validated JSON so corruption can't propagate. The Observer → Reflector pattern (extract 9 categories of facts from generated text, then emit validated state deltas) is a clean architectural pattern you could slot between your draft agent and fixer chain.

## Why I think it's worth your attention
This is the closest open-source architecture to what you're already building. It independently arrived at fixer-chain + auditor + revision-loop, validating your design. But it added the truth-file layer you haven't built yet, and it works.

## What I will do (safety rule)
I won't install this. I'll deep-dive the source on GitHub, extract the truth-file architecture and audit rubric patterns, then build native versions adapted to your existing fixer pipeline. Briefing with implementation sketch will follow when you approve.

🔗 https://github.com/Narcooo/inkos
