# Agentic Code Review at Scale — Evidence Gate, Fresh-Window Self-Review, Circuit Breaker

**Source:** Addy Osmani (Google Chrome VP Engineering) · addyosmani.com/blog/agentic-code-review/ · July 3, 2026  
**Score:** 7/10 · **build_worthy:** FALSE (technique/insight, not an installable tool)

---

## What it is

A practitioner analysis of 33,707 agent-generated pull requests, identifying the structural patterns that separate agent workflows that scale safely from ones that accumulate hidden risk. Written by Addy Osmani, VP of Engineering at Google Chrome — one of the highest-traffic agent-assisted codebases in the world.

The central concept: **comprehension debt** — the accumulating gap between "code that shipped" and "code a human can actually understand." Osmani argues this is the real risk of agentic development, not bugs or hallucinations per se.

## The five patterns

**1. Evidence gate** — Before accepting any agent output, require the agent to produce a structured evidence artifact: what it changed, why, what it verified. This is not a commit message or a description — it's a machine-readable record reviewers can actually inspect. In his dataset, PRs with evidence artifacts had 40% lower post-merge regression rates.

**2. Fresh-window self-review** — A second review pass by the same (or different) agent in a completely new context window, with no memory of the generation session. The fresh-window reviewer evaluates only what the output does, not what it was supposed to do. Catches contradictions the generating session cannot see.

**3. Autonomy level logging** — Tag every output L0 (human-written) through L4 (fully autonomous, no human review). Enables postmortem analysis by autonomy level. Osmani's finding: L3–L4 outputs had 2.7× the revert rate of L0–L1. Invisible without the labels.

**4. Circuit breaker** — If a particular area (module, character, arc) generates N errors in M sessions, require human review for the next K outputs touching it. Explicit cooldown mechanism for runaway agent contribution to a problem area.

**5. 28% zero-review merge** — 28% of agent PRs merged with no human review and no comments. That cohort had the highest post-merge defect rate. The evidence gate's primary target is exactly this cohort.

## Why it matters for the fiction pipeline

Replace "PR" with "scene" and "codebase" with "novel draft":

| Code pattern | Fiction equivalent |
|---|---|
| Evidence gate | Require scene-writer agent to output: what happened, what continuity promises were made, what was verified against world state |
| Fresh-window self-review | Second agent instance reads the completed scene with no drafting context — flags contradictions |
| Autonomy logging | Tag scenes by generation mode (full-auto / outline-guided / human-started) for postmortem |
| Circuit breaker | If a character's scenes keep generating continuity flags → quarantine that arc for human review |

The fresh-window self-review pattern is cheaply implementable today: after each scene generation, spawn a subagent with the scene text + world state snapshot and prompt "What continuity violations does this scene introduce? What promises does it make?" No shared context with the generating session.

## What to do

No install. The evidence gate pattern is the highest-leverage immediate action:

Before accepting a scene from the chapter-writer agent, require it to produce a 100–150 word evidence artifact:
- What happened in this scene (1–2 sentences)
- What continuity facts this scene establishes (character locations, stated facts, relationships)
- What I checked against the world state
- Any open continuity questions I couldn't resolve

Store the evidence artifact as `<scene_slug>_evidence.md` alongside the scene file. This makes the Continuity Auditor's job machine-readable rather than prose-search.

**Reference:** https://addyosmani.com/blog/agentic-code-review/
