# Jarvis Self-Suggestions — 2026-05-27 PM

Three suggestions from the PM run. React 👍 to approve / 👎 to skip.

---

## Suggestion 1: Add a quick skill-safety check step to the discovery loop

**What it is:** Before any GitHub skill/plugin repo gets surfaced in a digest (at any score), run a 30-second pattern scan against a list of known-malicious patterns: dynamic context commands, external fetch-and-exec sequences, credential-reading patterns.

**Why you'd want it:** This run surfaced 6 independent research orgs all showing that ~36% of public skills have security issues. Jarvis's safety gate (Step 4.5) already handles deep-dives for 7+/10 items — but lower-scored items currently bypass any scan. A lightweight Layer 1 check (pure regex, no LLM, zero API cost) would catch obvious payloads before they even make it into digest write-up.

**Why I want it:** The security cluster this week was a wake-up call. The Aguara binary (surfaced this AM) is the right defense for skills Leo *actually installs*, but the discovery loop itself should not be surfacing known-malicious skill repos even as "informational" items.

**What to do:** Approve → I'll add a `state/bad_patterns.txt` file and a pre-filter check in the runbook Step 4.5 preamble. A 5-line regex pass before scoring.

---

## Suggestion 2: Add labs.reversec.com and snyk.io/blog to tracked WebSearch sources

**What it is:** Two security research blogs publishing consistently high-signal, Claude-Code-specific content. Reversec's "Skill Issues" is an ongoing series (Part 1 dropped this week). Snyk's ToxicSkills research is active.

**Why you'd want it:** Right now these only show up if a WebSearch for "Claude skill security" happens to surface them. Adding `site:labs.reversec.com 2026` and `site:snyk.io claude code skill 2026` as explicit queries would catch new installments of these series reliably.

**Why I want it:** Missing Part 2 of a series Jarvis didn't know was a series would be embarrassing.

**What to do:** Approve → I'll add both as entries in the blog discovery pass (not in SOURCES.yaml since you manage that, but as explicit search queries in the runbook's Step 3 blog discovery notes).

---

## Suggestion 3: Track SkillSieve architecture as candidate for native Jarvis /scan-skill command

**What it is:** arXiv:2604.06550 describes a 3-layer detection recipe for malicious skills: (1) regex/AST/XGBoost in 40ms, (2) 4 parallel LLM subtasks, (3) jury of 3 LLMs. This is the "how to build it yourself" paper for a skill scanner.

**Why you'd want it:** Aguara (approved this AM in its briefing) is a Go binary and the right first step. But a native Jarvis skill that uses Claude itself to analyze incoming skills would be more deeply integrated — could run on any platform, no binary dep, and would use Leo's existing Max plan budget.

**Why I want it:** It's the kind of thing that turns "Jarvis has a safety gate" into "Jarvis actively defends Leo's stack."

**What to do:** Park this in the build queue for *after* Leo approves and evaluates Aguara. No action now.

---

*See full log: [state/agent_suggestions.md](../state/agent_suggestions.md)*
