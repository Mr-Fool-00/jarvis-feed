# Briefing: Loop Engineering — Boris Cherny's Loops-Over-Prompts Pattern

**Date:** 2026-06-19
**Score:** 7/10
**Build verdict:** BUILD_WORTHY ✅
**Suggested skill:** `/loop-engineer`

---

## The Idea

Boris Cherny (head of Claude Code at Anthropic) and Addy Osmani (Google Chrome DevRel) both published on the same concept in June 2026, independently arriving at the same architectural shift:

> **"I no longer prompt. I write loops."** — Boris Cherny, The New Stack interview, June 2026

A **loop** is a tick-based program:

```
while not done:
    output = agent.prompt(task + context)
    done, feedback = verifier.check(output)
    if not done:
        context = context + feedback
        continue
return output
```

The loop IS the product. The prompts inside it are implementation details.

Addy Osmani's framing: **agent harness engineering** — the skill is not writing good prompts, it's building the harness that knows when "good enough" has been achieved programmatically.

---

## Why This Is Build-Worthy for Leo

Leo's current fiction pipeline is already loop-shaped:

1. Generate scene/chapter
2. Evaluate (manually or via critic agent)
3. Revise if needed
4. Repeat until approved

But steps 2–4 are currently manual or ad-hoc. A `/loop-engineer` skill could:
- Accept a task description + exit condition
- Scaffold the tick-based re-prompt loop
- Wire a verifier agent that checks the exit condition
- Return only when the verifier passes (or hits a max-iteration safety cap)

For Jarvis specifically: Jarvis IS a loop — 12-hour ticks, check for new content, decide if something needs surfacing, loop. The loop-engineering pattern makes this explicit and inspectable.

---

## Implementation Sketch

```
/loop-engineer
  --task "Write a 3000-word chapter opening that establishes <setting>"
  --verifier "Check: does the scene establish location, time of day, and protagonist POV within the first 500 words?"
  --max-iterations 5
  --model claude-sonnet-4-6
```

Internally the skill would:
1. Run the primary agent with the task
2. Pass the output to the verifier agent with the check criteria
3. If verifier returns PASS → done, surface output
4. If FAIL → extract feedback, re-run primary with `[prev output] + [verifier feedback]` in context
5. Cap at max-iterations, surface best output with iteration log

---

## Sources

- Boris Cherny interview: The New Stack, June 2026 — "I no longer prompt, I write loops"
- Addy Osmani: addyosmani.com/blog — "Agent Harness Engineering" (June 2026)
- Related: Addy Osmani "Long-Running Agents" (May 2026) — context for why loops beat one-shot prompts at scale

---

## What Leo Needs to Decide

1. **Is this the right abstraction for the fiction pipeline?** The loop pattern is clean, but Leo's fiction work may need human-in-the-loop checkpoints that a fully automated verifier can't replace.
2. **What's the verifier agent's criteria?** "Good enough" for a chapter opening is subjective. The verifier needs concrete, programmatically checkable criteria (word count, POV consistency, checklist items) not aesthetic judgement.
3. **Safety cap:** Max iterations = 5 is a reasonable default. Too low and the loop gives up; too high and it burns tokens on marginal improvements.

No skill has been created. This briefing is the pre-approval deep-dive. Leo can decide to proceed or park it.

---

*Safety gate: No third-party code. This is a pattern, not an installation. No MCP to install. Building the skill from scratch after Leo's approval.*
