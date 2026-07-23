# Briefing: Claude Security Plugin Beta — Official Multi-Agent Vulnerability Scanner for Claude Code

**Date:** 2026-07-23 (PM run)
**Score:** 9/10
**Category:** Anthropic official release · Claude Code plugin · multi-agent
**Build verdict:** BUILDABLE — enable on jarvis-feed; extract independent-reviewer pattern for Jarvis multi-agent flows

---

## What it is

Anthropic shipped Claude Security as an **official Claude Code plugin** in public beta on July 22, 2026.

It's a multi-agent vulnerability scanning pipeline that runs directly in your Claude Code terminal session. It's not a linter or static analysis tool — it's a coordinated team of Claude agents:

1. **Architecture mapper** — reads your codebase, builds a threat model
2. **Vulnerability hunters** — trace data flows across files, identify flaws
3. **Independent reviewer** — checks every finding before it hits the report (a separate agent that didn't write the findings)
4. **Patch generator + test runner** — writes suggested fixes and runs your project's test suite against them before handing the patch to you

You can scan your full codebase OR just a commit diff, PR diff, or single commit. Run it from the terminal before pushing.

**Supported vulnerability types (25+):** injection flaws, authentication bypasses, memory corruption, logic errors, race conditions, insecure deserialization, path traversal, and more.

---

## Setup

**Requirements:**
- Claude Code v2.1.154+
- Paid plan (Pro / Max / Team / Enterprise)
- Python 3.9.6+
- Dynamic Workflows enabled
- Git (for change scans; full scans work in unversioned dirs)

**Enable:** Admin console → claude.ai/admin-settings/claude-code → Claude Security → Enable beta

**Docs:** https://code.claude.com/docs/en/claude-security

**Official announcement:** https://claude.com/blog/claude-security-public-beta

---

## Why this matters for Leo / jarvis-feed

**Immediately actionable:**
```bash
# After enabling beta in admin console, from inside jarvis-feed:
claude security scan --diff HEAD~5..HEAD
# or: full repo scan
claude security scan
```

The commit-diff mode (`--diff`) is exactly what you'd want to run pre-push on jarvis-feed. It reviews exactly what changed and flags issues in context.

**Architectural signal — the independent-reviewer pattern:**

The most interesting part of Claude Security isn't the vulnerability scanner; it's the design choice to use a **separate, independent agent to review every finding and every patch** before surfacing it to the user.

This is the adversarial-verify pattern applied to security:
- Agent A finds a bug
- Agent B (blind to Agent A's process) reviews whether the bug is real
- Agent C reviews the proposed patch + runs tests

This design prevents false positives and hallucinated vulnerabilities from shipping to users. **The same pattern should be in Jarvis's multi-agent flows:** when one agent produces output (a chapter draft, a continuity check, a skill recommendation), a second independent agent should review it before it reaches Leo.

Actionable for Leo's book pipeline:
- Add an independent "continuity reviewer" agent that reviews chapter-agent output
- Add an independent "factuality checker" agent for research-intensive chapters
- Extract this as a `verify-output.skill.md` that can be injected into any pipeline

---

## Build sequence (priority order)

1. **Today:** Enable Claude Security beta in admin console (5 minutes)
2. **Next:** Run `claude security scan` on jarvis-feed. See what it flags — good calibration for how the tool reasons about code vs. what you'd expect
3. **Then:** Add a pre-push hook to jarvis-feed that runs `claude security scan --diff` on staged commits
4. **Longer term:** Extract the "independent reviewer" pattern into a reusable Jarvis skill template

---

## Safety gate

This is an **official Anthropic product**, not third-party code. Safety gate is NOT required. No external code execution except the project's own test suite (which you already trust).

The Claude Security agents read your code but do not execute it (they analyze it). Patch suggestions require your explicit review and `git apply`.
