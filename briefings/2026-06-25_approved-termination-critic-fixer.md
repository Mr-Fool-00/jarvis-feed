# Adversarial Critic/Fixer with APPROVED Termination — 7/10

## What it is

A writing workflow published today that uses two separate AI agents in a loop: one that criticizes a draft and one that fixes it. The loop keeps going until the critic decides the writing is good enough and sends a special signal — the word "APPROVED" — to stop. No iteration caps, no timers. It stops when quality is actually achieved.

## Why you'd want it (specific to your stack)

Your chapter writing pipeline currently doesn't have a clean exit condition for quality gates. When a judge agent reviews a chapter, the loop either runs a fixed number of times (3 tries, then give up) or uses a vague score threshold. The APPROVED signal pattern is simpler and sharper: the critic just outputs either "APPROVED" or a critique paragraph — the orchestrator checks if it starts with "APPROVED" and exits. If it doesn't, fixer runs, then critic runs again. Repeat until APPROVED. This would tighten the chapter quality gate in your writing pipeline and eliminate the "gave up after 3 tries" failure mode.

## Why I think it's worth your attention

The APPROVED signal is one of those things that's obvious in hindsight but nobody does consistently. Most judge loops I've seen cap at N iterations. This approach makes the critic the termination authority, not the orchestrator — which is the correct design. It's also about 20 lines of orchestration code.

## Implementation blueprint (from deep-read of the source repo)

**Three files to create:**

**`.claude/agents/chapter-critic.md`** (YAML frontmatter):
```yaml
name: chapter-critic
description: Adversarial QA agent that reads a chapter draft and produces a harsh, actionable critique. Does NOT edit files — read-only analysis only.
tools: Read, Grep, Glob
model: opus
effort: high
```
System prompt defines the hard gates for chapters (prose quality, pacing, character voice, continuity). Critic writes its verdict to `quality_reports/[chapter]_critic_round[N].md`. Verdict must be one of: `APPROVED` (zero critical, zero major, ≤3 minor issues), `NEEDS REVISION` (any critical/major), `REJECTED` (hard gate failure). **Critic is read-only by design** — it has no incentive to downplay problems because it cannot fix them.

**`.claude/agents/chapter-fixer.md`** (YAML frontmatter):
```yaml
name: chapter-fixer
description: Implements fixes from the chapter-critic agent. Applies changes to the chapter draft in priority order: Critical → Major → Minor.
tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
effort: medium
```
Fixer reads the critic's quality report, applies fixes, writes a fix report. Using Sonnet for fixing saves ~40–60% vs. running both on Opus, with no quality loss on the implementation step.

**`.claude/skills/chapter-critic.md`** — the orchestration loop:
```
1. Invoke chapter-critic → writes quality_reports/[chapter]_critic_round[N].md
2. Read report, check verdict string
3. If APPROVED → return approved draft, done
4. If NEEDS REVISION or REJECTED → invoke chapter-fixer with critic report path
5. Increment round counter
6. If round == 5 → exit with best draft + warning (5-round fallback cap)
7. Else → go to step 1
```

**Key design insight:** The critic writes findings to disk; the fixer reads from disk. They never share a context window — this is what prevents self-consistency bias. The skill is the orchestrator; it just checks the verdict string in the report header and routes accordingly.

**Model routing:** Opus for the critic (judgment), Sonnet for the fixer (execution). Reserve the expensive model for the call that actually matters; the mechanical implementation step doesn't need it.

🔗 https://www.blog.brightcoding.dev/2026/06/25/stop-writing-papers-alone-this-claude-code-academic-workflow-changes-everything
🔗 https://github.com/pedrohcgs/claude-code-my-workflow
