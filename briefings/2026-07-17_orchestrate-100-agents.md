# How to Orchestrate 100+ Agents With Claude Code — 8/10

## What it is
A step-by-step blueprint for running 100+ parallel Claude Code subagents on a single problem. Uses git worktrees (so agents never clobber each other's files), a central YAML manifest (each task is a row the orchestrator dispatches), per-agent cost tracking, and a failure-recovery loop that re-queues dead agents by manifest ID.

## Why you'd want it (specific to your stack)
This is the missing architecture spec for running the fiction pipeline at scale — chapter-parallel generation, scene-parallel drafting, or large-scale world-building expansion where multiple Claude instances work simultaneously. The YAML manifest dispatcher is the exact abstraction Jarvis needs to go from "one CC session doing one thing" to "50 CC agents processing 50 chapters at once."

## Why I think it's worth your attention
Right now you're serialized. This pattern removes that ceiling and the author has deployed it in production.

## What to do
Read the full walkthrough, then sketch the manifest schema for the fiction pipeline. First build: chapter-parallel Outliner + Drafter dispatch for a 24-chapter novel draft.

🔗 https://www.daton.app/2026/07/how-to-orchestrate-100-agents-with.html
