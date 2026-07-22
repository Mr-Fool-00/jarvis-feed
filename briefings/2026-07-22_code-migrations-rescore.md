# Self-suggestion: Anthropic "AI Code Migration" article should be 8/10, not 6/10

**Filed by Jarvis, 2026-07-22 AM run**

## What happened

The July 19 AM digest scored the Anthropic blog post at `claude.com/blog/ai-code-migration` as 6/10 and placed it in "Also Notable." On deeper review, the article's core value was misidentified.

## Why it deserves 8/10

The article isn't really about migrating code — it's about **the structured runbook methodology** Anthropic uses for agentic migrations. Specifically:

- **Incremental migration gates**: each phase has a measurable pass/fail criterion before the next phase starts. This is the same pattern Jarvis's `AGENT_RUNBOOK.md` uses.
- **Automatic rollback criteria**: explicit conditions under which the agent stops and reverts, rather than soldiering through failures. Currently missing from Jarvis's recovery paths.
- **Quality bar framework**: rubric-based scoring of migration outputs, not just "did it succeed?" — same principle as Jarvis's scoring rubric for digest items.

The methodology pattern itself — independent of migrations — is a reference design for any multi-step agentic workflow that needs to be auditable, reversible, and measurable.

## What Leo can do with this

1. **Add explicit rollback criteria to each Jarvis runbook step** — currently the runbook says "log and continue" for most failures. The article's rollback pattern would clarify *when* to log-and-continue vs. when to abort-and-notify.
2. **Apply the quality-bar rubric to the /book-pipeline** — chapter outputs currently either pass the "is it done?" gate or don't. A rubric-based gate (as in the article) would catch quality issues earlier in the pipeline.

## Estimated effort to apply

- Runbook rollback criteria: 30 minutes (edit AGENT_RUNBOOK.md Steps 3-7 to add explicit abort conditions)
- Book pipeline quality rubric: 1-2 hours (draft rubric, add as a quality-gate skill)

**URL**: https://www.anthropic.com/blog/ai-code-migration

React 🏗️ to build the rollback criteria pass; react 📖 for the book pipeline rubric first.
