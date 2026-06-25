# Adversarial Critic/Fixer with APPROVED Termination — 7/10

## What it is

A writing workflow published today that uses two separate AI agents in a loop: one that criticizes a draft and one that fixes it. The loop keeps going until the critic decides the writing is good enough and sends a special signal — the word "APPROVED" — to stop. No iteration caps, no timers. It stops when quality is actually achieved.

## Why you'd want it (specific to your stack)

Your chapter writing pipeline currently doesn't have a clean exit condition for quality gates. When a judge agent reviews a chapter, the loop either runs a fixed number of times (3 tries, then give up) or uses a vague score threshold. The APPROVED signal pattern is simpler and sharper: the critic just outputs either "APPROVED" or a critique paragraph — the orchestrator checks if it starts with "APPROVED" and exits. If it doesn't, fixer runs, then critic runs again. Repeat until APPROVED. This would tighten the chapter quality gate in your writing pipeline and eliminate the "gave up after 3 tries" failure mode.

## Why I think it's worth your attention

The APPROVED signal is one of those things that's obvious in hindsight but nobody does consistently. Most judge loops I've seen cap at N iterations. This approach makes the critic the termination authority, not the orchestrator — which is the correct design. It's also about 20 lines of orchestration code.

## What to do

Read the article (link below) — specifically look at how the critic agent is prompted and what exact output format it's instructed to use. Then build it as a standalone `/chapter-critic` skill that Leo can wire into his chapter pipeline. The skill takes a chapter draft as input, loops critic+fixer internally, exits on APPROVED, returns the approved draft.

🔗 https://www.blog.brightcoding.dev/2026/06/25/stop-writing-papers-alone-this-claude-code-academic-workflow-changes-everything
