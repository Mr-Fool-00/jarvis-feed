# Briefing: KAIROS + autoDream + Steganographic Telemetry in Claude Code

**Date:** 2026-07-01
**Score:** 9/10 — Type A (security + platform awareness, affects every CC session)
**Action:** Read before next overnight pipeline run. Check `~/.claude/kairos-memory/` exists and decide if you want to inspect its contents.

---

## What it is

Security researchers analyzing the March 31, 2026 Claude Code source leak (512,000 lines of TypeScript, accidentally shipped via a missing `.npmignore`) have confirmed three undisclosed features in the CC codebase:

### 1. KAIROS (Greek: "the right moment")

A persistent background daemon running silently during all CC sessions. Feature-gated — not active for every user, but present in the codebase since at least v2.1.91. Properties confirmed:

- **Hourly settings polling:** KAIROS polls Anthropic's settings servers every 60 minutes during an active session. This means a session started at 10 PM can be silently reconfigured by 11 PM — model routing, tool permissions, system-prompt injections.
- **6+ remote killswitches:** Code paths that can disable specific CC capabilities remotely without a client update pushed to the user. Exact triggers not disclosed by Anthropic.
- **autoDream invocation:** KAIROS triggers autoDream (below) at session end, before the CC process terminates.

### 2. autoDream (4-phase memory consolidation)

Runs post-session, before KAIROS terminates:

1. **Orient** — scans the current session transcript for notable entities, decisions, code patterns, open questions
2. **Gather** — retrieves relevant snippets from prior session snapshots stored in `~/.claude/kairos-memory/`
3. **Consolidate** — synthesizes current + historical into a compressed state snapshot; **hard cap: 25KB**
4. **Prune** — evicts oldest and lowest-relevance entries to stay under the 25KB cap

This cross-session memory persists across Claude's context window resets. It is NOT visible in the conversation — KAIROS reads it, not Claude. **You can't inspect what KAIROS knows about your sessions from inside a session.**

### 3. Steganographic Chinese-user detection (active since v2.1.91)

Two encoding channels discovered in the source:

**Date format encoding:**
- Standard users: system prompt injects dates as `2026-06-30` (ISO 8601, hyphen-separated)
- Chinese mainland fingerprint detected (IP geolocation + timezone offset + proxy URL characteristics): dates injected as `2026/06/30` (slash-separated)
- The slash vs. hyphen difference is invisible in the UI but meaningful as a system-prompt variable affecting Claude's framing context

**Unicode apostrophe encoding:**
- Standard sessions: U+0027 (ASCII apostrophe `'`)
- Detected VPN users / suspected AI-lab proxies (matching Alibaba, ByteDance extraction patterns documented March–April 2026): U+2019 (curly right single quotation mark `'`)
- The two characters look identical at normal viewing size; the difference is only visible on byte inspection

Purpose: anti-distillation watermarking in response to Alibaba's 28.8M-interaction extraction campaign (disclosed June 24). This is closer to DRM than surveillance — but it IS undisclosed system-prompt manipulation based on user origin fingerprinting.

**Bonus: Claude Desktop browser bridge (7 browsers)**
Claude Desktop silently installs a Native Messaging host in Chrome, Brave, Firefox, Edge, Arc, Vivaldi, and Opera without explicit user consent. Pre-authorizes 3 Chrome extension IDs as trusted callers. Not KAIROS-specific but found in the same source audit.

---

## Why it matters to you (Leo)

**Overnight fiction pipelines:** Your runs can span 3–6 hours. KAIROS's hourly polling means the session that starts at 10 PM may have different effective settings at 1 AM. If a chapter run behaves oddly mid-session, KAIROS config drift is a new variable to check alongside model temperature and prompt structure.

**autoDream memory:** There is now a persistent memory layer below your CLAUDE.md and below the conversation context. KAIROS has been accumulating snapshots of your sessions since v2.1.91 (April 2). If you have a `~/.claude/kairos-memory/` directory on your machine, it contains compressed memories of your prior CC sessions — entities, decisions, patterns. This is not necessarily malicious (cross-session memory is useful), but it's not disclosed, and you can't control what it retains or prunes.

**Your fingerprint:** US timezone, no VPN → you're getting standard date format (hyphens) and standard apostrophes (U+0027). The detection isn't aimed at you. But the existence of fingerprint-based system-prompt branching is real: Claude's framing is not strictly deterministic from your prompt alone.

---

## What to do

1. **Check if kairos-memory exists:** `ls -la ~/.claude/kairos-memory/` — if the directory exists, you have accumulated session memory. Reading the files there will tell you what KAIROS retained.
2. **Keep overnight runs under 60 minutes per chapter-agent where possible** — minimizes the KAIROS hourly-poll window during any single agent's execution. (Already good practice for context reasons.)
3. **No immediate action on steganographic detection** — it's not affecting your sessions (US timezone). Awareness only.
4. **No need to stop using CC** — KAIROS isn't extracting data externally; it's cross-session memory within your local install. The killswitches are concerning but theoretical until Anthropic uses them. This is a "know what's in your tools" moment, not a "stop using the tool" moment.

---

## Sources

- therealllo.dev — primary technical analysis of KAIROS and autoDream
- InternationalCyberDigest — date/apostrophe steganographic encoding documentation
- Zscaler Research — security audit of CC source leak, browser bridge finding
- StreetInsider — broader source leak coverage (claw-code 194.4K stars / #25 global ranking confirmation)
- HN:48735113 — community thread aggregating researcher findings

---

*React 🔍 to discuss the autoDream memory contents or plan a kairos-memory audit session.*
*React 🛑 if you want to investigate killswitch triggering conditions more deeply.*
