# How to Use Claude Code Like Its Builders — Boris Cherny + Cat Wu — 7/10

## What it is

An Every.to podcast where Claude Code's lead engineers (Boris Cherny and Cat Wu at Anthropic) explain how they actually use the tool they built. Not theory — they're describing their own daily workflows.

The three things worth knowing:

1. **Plan mode before anything complex.** Hit Shift+Tab in the CLI before multi-file work. Claude maps out the plan, you review and align, THEN it executes. Doubles or triples success rates on hard tasks. Cherny says this is the #1 habit he'd teach new users.

2. **Shared settings.json in the repo root.** If you work on the same codebase repeatedly (writing pipeline, Jarvis), put a settings.json that pre-approves common commands and blocks risky ones. Everyone (or every Claude session) inherits it. This is what Leo's .claude/settings.json already is — worth knowing the builder endorses this exact pattern.

3. **Multi-subagent code review = parallel critics, not linear chain.** Cherny spawns 4+ subagents for code review. Each checks a different dimension simultaneously. Then additional subagents critique each other's findings. This is your Council pattern applied to code review — and it's what the head of Claude Code does on his own code.

## Why you'd want it (specific to your stack)

The plan-mode-first habit maps directly to your chapter generation pipeline. Before a complex multi-chapter run, plan mode = Claude builds the scene-by-scene scaffold, you approve it, THEN it writes. This would reduce mid-run course corrections that cost tokens and time. The parallel-reviewer pattern (point 3) is directly applicable to your chapter review stage — instead of FIXER_01 → FIXER_02 → ... → FIXER_14 in series, you'd run them in parallel as a council. Faster, same coverage.

## Why I think it's worth your attention

These aren't tips from a power user — they're tips from the person who designed the tool's architecture. If Cherny does it this way, it's probably because the tool was designed to work best this way.

## What to do

Enforce plan mode for all multi-chapter pipeline runs (add it to CLAUDE.md as a required first step). Review whether your fixer chain could parallelize — if the fixers are independent (style vs. plot vs. voice), they could run concurrently instead of sequentially.

🔗 https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it
