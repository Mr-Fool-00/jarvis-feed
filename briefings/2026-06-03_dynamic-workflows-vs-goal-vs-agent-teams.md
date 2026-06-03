# Dynamic Workflows vs /goal vs Agent Teams — 7/10

## What it is
A practical decision guide explaining when to use each of Claude Code's three orchestration tools: `/goal`, Dynamic Workflows, and Agent Teams. Published June 1-2, 2026 by MindStudio — a high-signal Claude practitioner blog. The short version: these three tools solve three different shapes of problem, and using the wrong one costs you tokens without improving results.

## Why you'd want it (specific to your stack)
You just got Dynamic Workflows 6 days ago and now have three ways to run multi-agent work. Your fiction pipeline needs all three at different points — here's the breakdown for your exact use case:

- **`/goal`** → "Rewrite Chapter 7 with better continuity" (you know the job, one agent does it, keep going until done). Cheapest.
- **Dynamic Workflows** → "Find every place in the novel where the protagonist's voice drifts" (you don't know what you'll find, parallel agents search simultaneously and converge). Medium cost. This is the consistency/quality-check pass.
- **Agent Teams** → "Write chapters 4, 5, and 6 in parallel" (genuinely independent creation tasks that don't need each other's context). Most expensive.

The AM digest today said single-agent beats multi-agent on quality per token. This doesn't contradict that — Agent Teams is for throughput (parallel chapters), not for quality on a single chapter. /goal for quality, Agent Teams for throughput, Dynamic Workflows for investigation.

## Why I think it's worth your attention
This is the clearest practical split of three tools that have been somewhat blurry since Dynamic Workflows landed. The depth/width framing (goal = depth-first, dynamic workflows = width-first) is the mental model you want before architecting your summer fiction pipeline.

## What to do
Read the comparison, then decide which of the three patterns each step in your fiction pipeline should use. The consistency/continuity check passes (your quality gate between draft and publish) are the clearest Dynamic Workflows use case in your stack.

🔗 https://www.mindstudio.ai/blog/dynamic-workflows-vs-goal-vs-agent-teams-claude-code
🔗 https://www.mindstudio.ai/blog/claude-code-goal-command-vs-dynamic-workflows (companion: depth vs width framing)
