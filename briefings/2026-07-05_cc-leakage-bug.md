# Briefing: Claude Code Session/Cache Leakage — Cross-Account Context Bleed

**Item:** GitHub anthropics/claude-code issue #74066  
**Source:** GitHub + Hacker News, July 5, 2026 (272 HN pts, ~180 comments at collection time)  
**Score:** 9/10 · **Digest:** 2026-07-05_PM · **Run:** PM  
**Action required:** Immediate — verify your own sessions; audit CCR run isolation

---

## What is being reported

Users are reporting that Claude Code is leaking session context or prompt cache data across account boundaries. The failure mode: one authenticated CC user is seeing fragments of another user's session — code context, conversation history, working memory — that they should not have access to.

The GitHub issue (#74066) is the canonical report thread. It hit 272 points on Hacker News within hours of surfacing, which is exceptionally high engagement for a CC-specific bug. The comment volume (~180) and point score both indicate that this is not a fringe report — multiple independent users are confirming the behavior.

---

## Why this is rated 9/10

This is not a UX regression. It is a **cross-account data exposure bug**. The potential impact:

- Session context from Account A surfaces during a session by Account B
- This could expose: code fragments, file paths, conversation history, task context, anything that was in the working session state when the cache was populated
- In shared compute environments (CCR, the remote execution environment Jarvis runs in), the isolation requirement is explicit — each session must be fully isolated from all other sessions

A 9/10 score reflects: (1) confirmed by multiple independent reporters, (2) category is security/privacy, not UX, (3) directly affects the environment this agent runs in (CCR is shared infrastructure).

---

## Your specific exposure surface

**High concern — CCR runs:**
Jarvis runs in CCR (Claude Code Remote execution environments). These are cloud-hosted containers provisioned for each session. If session cache is shared across container boundaries, every Jarvis run could potentially be exposing its session context to other CCR users, or receiving fragments of theirs.

Specific data that could leak from a Jarvis session:
- The GITHUB_PAT token (though this should only appear in git remote URLs, not plaintext session context)
- Source content from fetched articles (pre-summarized, could expose URLs or content fragments)
- `state/seen.json` fragments (low sensitivity)
- Any user-provided context from prior sessions if prompt cache is being shared

**Lower concern — local CC sessions:**
Local CC runs with only local process isolation and no shared cloud cache have a smaller attack surface. The leak reports appear to center on the cloud/CCR path.

---

## The technical mechanism (as understood from reports)

The most likely mechanism based on the reports: **prompt cache entries being served across account boundaries**. Claude's API-level prompt caching (which reduces latency and token cost for repeated context) uses cache keys derived from context content. If the cache key is not sufficiently scoped to the authenticated user, a cache entry written by User A could be served to User B if their prompt context matches the cache key prefix.

This is speculative based on community analysis in the HN thread — Anthropic has not published a root cause statement as of collection time.

---

## Immediate actions

**1. Read GitHub #74066** before the next Jarvis run. Check whether:
- Anthropic has confirmed the bug
- There is a workaround (e.g., an env var to disable prompt caching)
- The issue is already mitigated server-side

**2. Check recent Jarvis session transcripts** for anomalous context. If any session transcript contains code, content, or references that weren't in that session's inputs, that's a positive signal for contamination.

**3. Until the issue is resolved or confirmed safe:** Consider whether the Jarvis PM runs contain any context you would not want exposed to other CCR users. The current runs primarily fetch public web content and write to this repository — the sensitivity is relatively low. But if any session includes private context (personal notes, unpublished manuscript content, credentials in prompts), treat that as potentially exposed.

**4. Do not change the GITHUB_PAT** unless you confirm it appeared in a session transcript in plaintext. The token appears only in git remote URLs in bash commands, not in session context proper.

---

## Status tracking

- **As of 2026-07-05 PM run:** Open, unresolved, no Anthropic acknowledgment confirmed
- **Next check:** 2026-07-06 AM run — check #74066 for Anthropic response or closure
- **Resolution signal:** Issue closed with "fixed" label, or Anthropic publishes a security advisory

---

## Runbook note

Add to `AGENT_RUNBOOK.md` safety section: "If GitHub #74066 (CC session/cache leakage) is not yet resolved, treat each CCR session as potentially non-isolated. Do not include credentials, private manuscript content, or user-identifiable data in prompts. Prefer fetching public content only until confirmed safe."
