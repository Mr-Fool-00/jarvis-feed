# anthropics/cwc-long-running-agents — 9/10

## What it is

Anthropic just open-sourced the exact harness code they used at Code with Claude 2026. It's three short shell hook files + one subagent definition. Together they solve the two most annoying problems with running Claude on long tasks:

1. Claude declaring "I'm done!" before it actually checked its work
2. Claude losing its place when the context window resets mid-task

## Why you'd want it (specific to your stack)

Your writing pipeline has both of these problems right now.

**Problem 1 — premature success claims:** Your verify-15 relies on prompting Claude to not mark chapters as passing until all criteria are met. This breaks down in long sessions. The Default-FAIL Contract hook (`verify-gate.sh`) makes it structural: every criteria starts as `false` in a JSON file, and a pre-tool-use hook literally blocks Claude from writing a `true` until it has opened and read the actual evidence. Claude cannot cheat this. It's not a suggestion — it's a gate.

**Problem 2 — context rot on long books:** When Claude's context fills mid-chapter, it loses track of early decisions. The PROGRESS.md + `commit-on-stop.sh` pattern means Claude writes where it is before stopping, commits it to git automatically, and reads that summary first thing next session. Your 10-chapter book sessions would be way more coherent across session resets.

**Bonus:** The Fresh-Context Evaluator subagent (`agents/evaluator.md`) is exactly what your verify-15 should be — a separate Claude agent that never touched the draft, grades it from scratch, returns PASS or NEEDS_WORK. Right now your fixer chain runs in the same context as the writer. Clean separation = less drift.

## Why I think it's worth your attention

This is official Anthropic code. It's short, readable, and modular. Each hook is one standalone file with no dependencies. You'd adapt maybe 60-70 lines total to work with your pipeline. It's the fastest high-leverage upgrade available to your writing system right now.

## What to do

This is from Anthropic so there's no safety risk. You can look at the three files directly:
- `verify-gate.sh` — pre-tool hook that blocks premature success claims
- `agents/evaluator.md` — the fresh-context evaluator subagent
- `commit-on-stop.sh` — auto-commit backstop hook

When you're ready to build (post-finals): adapt these three to your chapter-validation flow. The integration is a weekend project.

🔗 https://github.com/anthropics/cwc-long-running-agents
