# Nested Sub-Agents in Claude Code — 9/10

## What it is
Claude Code v2.1.172 (released June 10) lets agents spawn their own agents — up to 5 levels deep. Before this, the rule was "sub-agents cannot spawn sub-agents." That rule is gone. Now a chapter-writer agent can kick off its own scene-writing or continuity-checking agents without you having to orchestrate that from the top.

## Why you'd want it (specific to your stack)
Your book pipeline today is flat: you orchestrate agents manually, one level down. With depth-2 nesting, the architecture that's been in the agent suggestions for weeks is now actually buildable:

```
book-coordinator
  → chapter-writer-1 (parallel)
      → scene-agent (writes scene 1)
      → continuity-checker (validates against story bible)
  → chapter-writer-2 (parallel)
      → scene-agent
      → continuity-checker
  → chapter-writer-3 (parallel)
```

Each chapter-writer runs at Sonnet 4.6 cost. Its scene-agents and checkers can run at Haiku cost. The 3W+3E+4R Barr Group architecture from the suggestions now has a native implementation path — you don't need to hand-wire the fixer loop from the outside.

Practical depth: aim for depth 2 (orchestrator → writers → fixers). Depth 3+ exists if you need it, but each level costs ~7× more tokens and disappears from the parent's context after the leaf summary.

## Why I think it's worth your attention
This is the primitive the fiction pipeline has been missing. You've been asking "how do I make chapter agents that run their own quality loops" since May. The answer is now built into CC itself, no workaround needed.

## What to do
1. Test a depth-2 chain on ONE chapter: `book-coordinator → chapter-writer → scene-fixer`
2. The chapter-writer's system prompt should say: "When you complete a scene draft, spawn a sub-agent with role=scene-fixer to validate continuity against story.md before outputting"
3. Measure token cost vs a flat run before committing the architecture
4. If it works, build `/book-pipeline` as a proper skill

🔗 https://github.com/anthropics/claude-code/releases/tag/v2.1.172
🔗 https://claudefa.st/blog/guide/agents/nested-subagents
