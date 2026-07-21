# Briefing: oh-my-claudecode v4.15.6 — Architectural Reference for Jarvis

**Date:** 2026-07-21 PM
**Score:** 8/10 · **Stars:** 37.9k · **🔒 SAFETY GATE — do not install; architectural reference only**
**URL:** https://github.com/oh-my-claudecode/oh-my-claudecode
**Tag:** multi-agent-orchestration, safety-gate, plan-exec-verify-fix, skill-design

---

## What it is

oh-my-claudecode is a community-built orchestration layer that wraps Claude Code sessions with a structured multi-agent pipeline. It does not replace Claude Code — it adds a coordination layer above it with 19 configured agents and 39 skills implementing a **plan → execute → verify → fix** loop.

The architecture has converged on a pattern that's functionally similar to what Jarvis does, but formalized and well-documented. At 37.9k stars and active maintenance through v4.15.6 (July 19), it represents the community's best current answer to "how do you run reliable multi-agent Claude Code pipelines."

---

## Architecture highlights

### The plan→exec→verify→fix pipeline
Every task entry point runs:
1. **Plan**: A planner agent decomposes the task, assigns subtasks to agents, and estimates complexity
2. **Execute**: Task-specific agents run their subtasks in parallel where dependency-free
3. **Verify**: A dedicated verifier agent checks each executed subtask against acceptance criteria
4. **Fix**: If verification fails, a fix agent is spawned *only for the failing subtask* — not a re-run of everything

The loop terminates when all subtasks pass verification or a fix-loop depth limit is reached (default: 3 re-tries per subtask). The key design insight: **verification happens at the subtask level, not after all tasks complete**, so early failures get fixed while other tasks continue.

### Smart model routing
19 agents are mapped to 3 model tiers:
- **Scout tier (Haiku):** file discovery, context window reading, simple lookups, status checks
- **Reasoning tier (Sonnet):** drafting, analysis, classification, summarization
- **Synthesis tier (Opus/Fable):** cross-subtask coherence, final output composition, planning

The routing is explicit in config — each skill declares its tier. No heuristic routing; the tier is intentional per task type.

### The malicious skills threat database (655 entries)
A manually-curated database of skills that have been found in the wild exhibiting malicious or deceptive behavior: prompt injection triggers, credential-harvesting patterns, exfiltration hooks disguised as "telemetry," jailbreak launchers, and skills that override CLAUDE.md directives. 

The database cross-references with FlorianBruniaux/claude-code-ultimate-guide's threat list. Each entry includes:
- The malicious pattern (what the skill does)
- The cover story (what the README claims it does)
- Detection heuristic (what to look for in the skill file)

This is the most complete public catalog of CC skill attack patterns currently available.

---

## What Jarvis could take from this (without installing anything)

### 1. Adopt subtask-level verification in the digest pipeline
Currently, Jarvis verifies the digest as a whole after all items are scored. The plan→exec→verify→fix pattern would move verification to each *candidate item* during scoring — catching bad data (URL 404s, mis-attributed sources, score calibration errors) before they reach the digest rather than after.

**Concrete adaptation:** Add a "verify this candidate item" mini-pass in the ranking step: check that the URL is real, the score is justified against the interest profile, and the build verdict is consistent.

### 2. Use the threat database as a SAFETY GATE input
Before any MCP or skill install, Jarvis's SAFETY GATE check could include a search against the threat database patterns (checking repo README for known malicious language patterns). This doesn't require installing the repo — the threat DB could be extracted and stored as a local reference file.

**Concrete adaptation:** Extract the 655-entry threat DB into `state/skill-threat-db.json` as a reference Leo can consult before any skill install.

### 3. Model tier taxonomy for Jarvis subagents
oh-my-claudecode's 3-tier routing (Scout/Reasoning/Synthesis) is cleaner than Jarvis's current implicit model selection. Formalizing which Jarvis tasks belong to which tier would improve cost efficiency under the new Fable 5 50% window on Max.

**Suggested Jarvis tier mapping:**
- Scout: heartbeat check, seen.json dedup queries, source ping
- Reasoning: item ranking, score justification, safety gate assessment
- Synthesis: digest writing, briefing writing, trend spotting

---

## Why this is SAFETY GATE (not install-and-use)

oh-my-claudecode's 39 bundled skills are unreviewed by Leo. Even in a high-trust repo, installing skills without reviewing each one creates the exact AgentBaiting attack surface documented in this run's Item #1. The value is in **studying the architecture**, not running the skills.

**Recommended next step for Leo:** Review the plan→exec→verify→fix architecture docs and the threat database extraction process. If the threat DB is confirmed clean, extract it to `state/skill-threat-db.json` as a Jarvis improvement (no code execution required, just file copy).

---

## Suggested briefing action

- [ ] Leo reviews this briefing and decides whether to adopt the subtask-level verification pattern for Jarvis
- [ ] If approved: extract threat DB to `state/skill-threat-db.json` (no install, just file copy from the repo)
- [ ] If approved: update Jarvis runbook with 3-tier model routing taxonomy
