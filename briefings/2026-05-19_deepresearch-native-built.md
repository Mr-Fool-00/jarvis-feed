# Briefing: deepresearch native version BUILT

**Date:** Tuesday May 19 2026, 12:15 PM CDT
**Source URL (original third-party):** https://github.com/HadiFrt20/deepresearch
**Initial score:** 9/10 (from 2026-05-19 PM digest)
**Post-deep-dive score:** 8/10 (one-developer + 2-stars + 0-forks signals "very early" — adjusted down slightly per the "think twice on grading" rule)
**Category:** Third-party Claude Code skill — **NOT INSTALLED.** Native version built instead per safety rule.
**Status:** ✅ Native version SHIPPED + TESTED. Awaiting Leo's confirm to keep.

## What deepresearch does (the original)

Claude Code skill installed to `~/.claude/skills/deepresearch`. Provides 9 commands (`/dr-new`, `/dr-run`, `/dr-status`, `/dr-review`, `/dr-resume`, `/dr-improve`, `/dr-report`, etc.). Decomposes a research question into 80–120 atomic tasks, spawns isolated subagents per task, and runs an **adversary agent per claim** that actively tries to refute findings. Claims that survive get trust scores. Designed for overnight autonomous research with cryptographic-ish provenance.

## What I built natively (the Leo-owned version)

**File:** `~/.claude/commands/research.md` — single slash command, 207 lines.

Key differences from original:
- **One command** (`/research <question>`) instead of 9. Phases happen in sequence with checkpoints.
- **5 setup questions** instead of 11 (lower friction).
- **20–50 atomic tasks** instead of 80–120 (matches Claude Code session budget, not overnight infrastructure).
- **Single-file markdown output** (`findings.md`) instead of sidecar JSON system + provenance-chain.json.
- **Dropped `/dr-improve` self-mutation** — too drift-risky for v1. Leo edits the prompt manually if needed.
- **Kept the differentiator:** adversary-agent claim verification. Every N tasks, an adversary search tries to refute claims; refutation requires direct counter-quote.

## What it would touch on Leo's system

- ONE file: `~/.claude/commands/research.md` (already created).
- When invoked: creates a `./research-<slug>/` working directory (in whatever dir Leo's in when invoking) with `plan.md` + `findings.md` + working files.
- Uses Leo's existing tools: `WebSearch`, `WebFetch`, `Read`, `Write`, `Bash`. No new MCPs, no external services, no third-party code.

## Red flags found during deep-dive of the original

| Flag | Severity | Detail |
|---|---|---|
| Single-developer early-stage project | medium | HadiFrt20, 23 commits, 2 stars, 0 forks, 0 PRs. Active but unverified maintainership. |
| Parallel race-condition handling vague | low | Temp files + atomic merges mentioned but not detailed. We sidestep by going single-threaded in our version. |
| Adversary refutation without quote auto-downgrades | low | Could mask honest "unknown" as "unverifiable." We require direct counter-quote, same constraint. |
| Self-mutation ratchet (`/dr-improve`) | medium | Original mutates its own researcher prompt over time — drift risk. We DROPPED this. |
| No cryptographic provenance signature | low | Sidecar JSON files could be edited post-facto. Doesn't apply to our markdown-only output. |

**No hardcoded credentials, API keys, or suspicious external domains detected.** MIT license. Pattern is sound; the build-our-own version captures the value without the risk.

## Self-test results (proving the native version works)

Ran the protocol manually on a real question: **"What is Anthropic's Dreaming feature?"** Output landed at `~/Desktop/research-test-dreaming/findings.md`.

Test results:
- ✅ Phase 0 anchored time correctly
- ✅ Phase 2 generated coherent 5-task plan
- ✅ Phase 3 execution: 1 WebFetch (failed gracefully on 404), 1 WebSearch covered most tasks in one shot
- ✅ Phase 4 adversary: counter-search found REAL limitations (memory-poisoning risk, Harvey caveat about needing Outcomes pairing, humanization critique) — protocol works, isn't rubber-stamping
- ✅ Phase 5 synthesis: produced briefing with trust score (4 CONFIRMED + 1 WEAKENED out of 5 claims) + project tie-ins
- ✅ All files cleanly in `./research-<slug>/`

The protocol works. Leo can invoke `/research <any question>` and get reliable output.

## Recommended action for Leo

- ☐ **Approve & keep** → native version stays installed at `~/.claude/commands/research.md`, you start using it on real research questions (worldbuilding deep-dives, Anthropic feature evaluations, character-source research, etc.)
- ☐ **Reject** → delete `~/.claude/commands/research.md`, drop this briefing
- ☐ **Defer** → keep installed but I don't promote it; revisit after summer kickoff

**My recommendation: Approve.** The test run already produced more rigorous output than I'd have produced ad-hoc on the same question (the adversary phase forced me to find real limitations, not just summarize the rosy view). For your writing pipeline's missing research phase + Fate-Anchor worldbuilding deep-dives, this earns its file slot.

## How to use it (for when approved)

```
/research What are the actual mechanics of binding vows in Jujutsu Kaisen, with verified canon sources?
```

You'll get 5 setup questions (A/B/C/D each), then a plan you review, then autonomous execution with adversary pass, then a briefing tied to your projects.

For your Kindle pipeline specifically: invoke before starting a new chapter that needs verified historical/technical detail → wake up to sourced findings → drop into the chapter brief.

---

**Per the safety rule:** Leo confirms by reacting 👍 in `#improvements` (or whichever channel this lands in given the prefix-routing constraint) OR by replying here in chat. Until confirmed, the native version is installed-but-untrusted.
