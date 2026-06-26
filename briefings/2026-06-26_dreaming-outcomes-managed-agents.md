# Anthropic Managed Agents: Dreaming + Outcomes — 8/10

## What it is
Anthropic shipped two distinct features for their Managed Agents platform on May 6 at CwC SF. **Outcomes** is an external grader agent — runs in a completely separate context window, knows nothing about how the task agent reasoned — that evaluates output against a rubric you write, then kicks the task back for revision if quality is below threshold. Creates automatic retry-until-quality loops. **Dreaming** is a scheduled background process that reviews agent sessions and memory stores after they finish, extracts cross-session patterns, and curates memory so agents improve over time without manual intervention. Harvey (legal AI) saw 6x task completion improvement after implementing Dreaming.

## Why you'd want it (specific to your stack)
Outcomes is the external quality gate your chapter writer is missing — right now the pipeline either caps at N iterations or uses heuristic checks; Outcomes exits only when quality is actually achieved, and Anthropic has measured +8-10% quality improvement at scale. Dreaming addresses the cold-start problem: each novel run currently starts from scratch; with Dreaming enabled, the writing agent would accumulate craft patterns and voice consistency lessons across sessions automatically.

## Why I think it's worth your attention
This is the same external-critic-in-separate-context pattern that BrightCoding (APPROVED signal, June 25) and MindStudio (3-drafts + external critic, June 25) independently landed on — Anthropic validated it at production scale with real numbers, and it's available now as a first-party managed feature.

## What to do
Read the announcement, then prototype Outcomes as the exit condition for your chapter quality gate: write a rubric describing what a "done" chapter looks like (voice consistency, scene completeness, no summary-writing), wire it as a grader against your chapter writer output, and measure whether it catches the cases where the current pipeline signs off too early.

🔗 https://claude.com/blog/new-in-claude-managed-agents
