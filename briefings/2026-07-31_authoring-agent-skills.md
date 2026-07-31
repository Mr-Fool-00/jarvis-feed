# Briefing: Authoring Agent Skills — Structured Composition for Claude Code Pipelines

**Date:** 2026-07-31
**Score:** 7/10
**Paper:** arXiv:2607.25032 (submitted July 27, 2026)
**Verdict:** READ-FIRST — directly relevant to your Claude Code skill authoring workflow; architecture vocabulary you're missing

---

## What it is

A research paper proposing and empirically evaluating a framework for treating Claude Code skills as first-class versioned artifacts rather than ad-hoc CLAUDE.md additions. The core claims:

- Skills should have explicit **dependency declarations** (what other skills, tools, or MCP servers they require)
- Skills should have **composition contracts** (what they expose, what they consume, what they modify)
- Skills should have **test harnesses** that can verify behavior before a session starts
- "Structured skill composition" — chaining skills with explicit data flow — outperforms monolithic context-dump approaches on complex multi-step agentic tasks

The paper evaluates this against both single-prompt baselines and existing skill libraries (likely including community repos like jeremylongshore's 3,069-skill collection and rohitg00's awesome-claude-code-toolkit).

🔗 https://arxiv.org/abs/2607.25032

---

## Why it's worth your attention

**You are actively doing what this paper studies.** Your pipeline has a growing set of skills spread across `.claude/agents/` and CLAUDE.md. You've hit the friction point where:
- Skills implicitly depend on other skills but that dependency isn't declared
- A skill change can break downstream skills in non-obvious ways
- Testing a skill requires launching a full Claude session

The "composition contract" pattern is the most immediately applicable piece: if each skill declares what context it needs, what it produces, and what side effects it has, you can:
1. Detect conflicts before a session starts (two skills both writing to the same file pattern)
2. Build skill chains where output from one becomes typed input to the next
3. Write unit tests for skills that don't require a live Claude session

The "dependency declaration" piece also helps with the proliferation problem — when you have 20+ skills, knowing which ones need each other for correct behavior becomes non-trivial.

---

## The community signal backing this

The paper arrived in the context of a community-wide convergence on skills-as-structured-artifacts:
- jeremylongshore/claude-code-plugins-plus-skills: 3,069 skills, 471 plugins with a `ccpi` CLI package manager — treating skills as installable packages
- rohitg00/awesome-claude-code-toolkit: 35 curated skills with explicit categories and dependency documentation
- haowjy/creative-writing-skills: domain-specific skills with focused scope (no dependencies on general-purpose infrastructure)

The trend is: skills get more useful when they're explicit about what they need and what they do. The paper is the theoretical foundation for what practitioners are building empirically.

---

## What to do

**Step 1:** Read the abstract and Section 2 (framework design) to understand the composition contract schema.

**Step 2:** Look at your current skills and identify which ones have implicit dependencies that aren't declared. A skill that assumes `/code-review` has been run first, or that a specific MCP server is connected, is already doing composition — just undocumented.

**Step 3:** For your two or three most-used skills (the ones in your daily pipeline), add a `## Dependencies` and `## Outputs` section to the skill file matching whatever schema the paper proposes.

**Step 4:** If the paper's test harness approach is tractable, try writing one test for your most complex skill. Even a minimal "does this skill produce the expected output format given this input" check would catch regressions.

**Build verdict:** READ-FIRST, then a focused skill-refactor afternoon. This isn't "build a new tool" — it's "apply a schema to skills you already have." Low implementation risk, high long-term value for pipeline maintainability.

🔗 https://arxiv.org/abs/2607.25032
