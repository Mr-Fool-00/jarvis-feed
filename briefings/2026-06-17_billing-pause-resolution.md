# Briefing: Anthropic Pool 2 Billing Pause — Concern Closed

**Date:** 2026-06-17 · **Item score:** 9/10 · **Build verdict:** INFORMATIONAL  
**Resolves:** agent_suggestions.md #20, #102, #106; June 15 AM digest item, June 16 AM digest item 5, June 16 PM digest item 5

---

## What happened

Anthropic announced on June 8, 2026 that a new billing structure would take effect June 15: "programmatic" Claude usage (claude -p, hooks, third-party schedulers, Agent SDK calls) would be separated into a "Pool 2" credit pool requiring a $200/month programmatic plan, distinct from the Max subscription ("Pool 1") used for interactive Claude.ai and Claude Code terminal sessions.

**On June 15 — the announced effective date — Anthropic paused the change before it went into effect.**

Reason given: developer feedback indicated the "interactive vs. programmatic" boundary was too ambiguous in practice. Many workflows blur the two categories (claude -p from a terminal, hooks firing on git commits, CCR sessions like Jarvis). The $200 programmatic tier would have immediately strained solo and indie developer accounts.

Current state as of June 17, 2026: the billing split has not taken effect. All usage continues on Pool 1 (your Max subscription). Anthropic will revise the model before re-introducing it and has committed to 30 days' notice.

---

## What this means for Jarvis and your CCR runs

CCR (Claude Code Routines) runs are NOT affected by Pool 2 billing:
- The pause means CCR billing classification was never enacted
- No retroactive charges
- Current Max subscription covers Jarvis runs as-is

Jarvis run frequency (2× daily, ~45-50 min each at Opus 4.8) remains within normal Max subscription usage. Nothing needs to change.

---

## What to watch going forward

1. **When Anthropic re-announces a billing revision** (30 days notice required): the key question will be whether CCR runs are explicitly classified as Pool 1 (interactive/subscription) or Pool 2 (programmatic). At that point, re-evaluate.

2. **The distinction that will matter**: CCR is a first-party Anthropic product, not a third-party scheduler. The revised billing model may treat first-party scheduling differently from external API callers. Watch the announcement language carefully.

3. **Polymarket/community signal**: if you see HN/Slack chatter about "Pool 2 is back" — that's the trigger to re-evaluate Jarvis run frequency and model selection.

---

## No action required now

- ✅ CCR billing: safe as-is
- ✅ Jarvis run frequency: no change needed
- ✅ Model selection (Opus 4.8): no change needed
- Optional: verify in your Anthropic billing portal that current usage is as expected

---

*This briefing closes the billing thread that was flagged in suggestions #20 (May 21), #102 (June 15 AM), and #106 (June 16 PM), and appeared as unresolved item 5 in two consecutive digests.*
