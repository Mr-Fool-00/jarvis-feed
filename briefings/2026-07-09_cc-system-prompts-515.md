# Briefing: Piebald-AI CC System Prompts — 515 Extracted Prompts, 231 CC Versions Documented

**Score:** 7/10 · **build_worthy:** ❌ NO (read-only knowledge asset)
**Source:** github.com/Piebald-AI/claude-code-system-prompts · July 8, 2026
**URL:** https://github.com/Piebald-AI/claude-code-system-prompts

*Safety gate: read-only reference repo, no executable code. ✓*

---

## What this is

A community repository that reverse-engineers and catalogs every system prompt CC sends to Claude under the hood. Expanded on July 8 from 350 → 515 prompts covering CC v2.0.14 through v2.1.205.

Included prompt categories:
- **Main system prompt**: the base instructions every CC session starts with — this is the thing your CLAUDE.md is partially overriding
- **Sub-agent prompts**: Plan, Explore, Task, and Orchestrator each have their own separate system prompts, distinct from the main one
- **Utility prompts**: commit message generation, PR description drafting, code review formatting
- **Hook execution context prompts**: what the model sees when running SessionStart/PreToolUse/PostToolUse hooks
- **CHANGELOG**: tracks which prompts changed between each of the 231 documented versions

---

## Why it matters

**For writing better CLAUDE.md**: the single biggest reason to look at this repo is to understand what the base system prompt already says before your CLAUDE.md additions layer on top. Most CLAUDE.md entries either:
- Redundantly repeat things CC already does (wasted tokens)
- Contradict the base prompt (causes model confusion / inconsistent behavior)
- Work with the base prompt's structure (ideal)

Reading the base prompt once tells you which category your existing CLAUDE.md falls into.

**For understanding sub-agent behavior**: the Plan, Explore, and Task sub-agents have different system prompts from the main agent. If you're orchestrating CC sub-agents and finding they behave differently from the main agent in ways you can't explain, the answer is probably in here.

**For debugging regressions between CC versions**: the CHANGELOG lets you see exactly which prompts changed between any two versions. When a behavior that was working in v2.1.195 breaks in v2.1.205, you can diff the relevant prompts to find the cause.

---

## Quick read recommendation

1. Read the main system prompt once (it's long but worth it)
2. Find the section that covers tool-call behavior and compare it to what your CLAUDE.md says about tools
3. Look at the Orchestrator prompt if you're building multi-agent setups — it's the most revealing about how CC thinks about sub-agent coordination
