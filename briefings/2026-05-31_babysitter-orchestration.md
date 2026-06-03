# Babysitter: Deterministic Multi-Agent Quality Gates — 7/10

## What it is

Babysitter is a tool that makes your AI agents follow a script — literally. You write a JavaScript file (the "Process file") that defines exactly what steps the agent can take and in what order. After every step, the agent has to pause and check with the Process file before it can do anything next. The agent cannot decide on its own what to do next. If a step doesn't meet the quality bar you set, it blocks and retries rather than continuing with bad output.

It also includes 2,000+ pre-built workflow templates (already-written Process files for common tasks) and claims 50-67% fewer tokens used because it stops agents from rambling through unnecessary steps.

1,000 GitHub stars. MIT license. Actively maintained (last version April 2026). No red flags found in the code review.

## Why you'd want it (specific to your stack)

Your fiction pipeline has a known problem: when you chain multiple writing or fixing agents together, quality can drift chapter by chapter and you don't always catch it until you're 10 chapters deep. Babysitter solves this at the infrastructure level — every step in the chain has to pass your quality gate before the next step runs. The Process file is yours. You define what "quality" means (word count range, chapter structure check, voice consistency score — whatever you want). The agent can't skip it or smooth-talk past it.

The 50-67% token savings matter directly to your Max plan budget. Fewer unnecessary agent loops = more capacity for the actual work.

## Why I think it's worth your attention

The "Process-as-Code" idea — where a JS file has more authority than the model itself — is a fundamentally different design from just prompting an agent to "check your work." The agent literally cannot proceed unless the code says so. That's not a soft guardrail, it's a hard one. For a summer of high-volume book production, that matters.

## What I will do (safety rule)

I won't install this. I've read the code, checked for red flags (none found), and the design is sound. But per our rule: you approve first, then I build a version we control from scratch, inspired by the Process-as-Code idea, using your existing hooks system. No third-party code in your stack.

React 👍 to this briefing in Slack if you want me to start building the native version. React 👎 to pass. React 👀 if you want more information first.

🔗 https://github.com/a5c-ai/babysitter
