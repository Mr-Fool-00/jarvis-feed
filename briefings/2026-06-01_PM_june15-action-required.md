# June 15 Billing Change: Action Checklist for Jarvis

**Urgency:** HIGH — 14 days to June 15. Missing the June 8 claim email means losing $200 in agent credit for the month.

**Context:** The Agent SDK billing split was first flagged on 2026-05-20 (briefing: `2026-05-20_agent-sdk-billing-split.md`). This briefing covers the new implementation details — specifically the claim mechanics, the model ID retirements, and what it means for Jarvis.

---

## What Changes June 15

Anthropic splits subscription usage into two pools:

- **Pool A (unchanged):** Interactive Claude Code in terminal/IDE, Claude.ai chat, Claude Cowork
- **Pool B (new monthly credit):** `claude -p`, Agent SDK calls, Claude Code GitHub Actions, third-party agents, Routines

**Jarvis runs as `claude -p` on a twice-daily Routine. It moves to Pool B.**

### Credit Amounts by Plan

| Plan | Monthly Agent Credit |
|------|---------------------|
| Pro | $20 |
| Max 5x | $100 |
| Max 20x | **$200** |

Credits: full API list rates, no rollover, reset monthly with billing cycle.

---

## Three Actions Before June 15

### Action 1: Claim Your Credit (June 8 email)

Anthropic sends the opt-in email **June 8**. You must click through once to activate your $200 credit. It auto-renews after that. If you don't claim it, programmatic usage either errors or bills at overflow rates from day one.

> **Set a reminder for June 8** to check your Anthropic account email.

### Action 2: Audit Jarvis for Retiring Model IDs

Two model IDs are retired on June 15. Any request using them after June 15 returns an error:

| Old ID | Replace With |
|--------|-------------|
| `claude-sonnet-4-20250514` | `claude-sonnet-4-6-20260217` |
| `claude-opus-4-20250514` | `claude-opus-4-7` or `claude-opus-4-8` |

**Audit targets:**
- AGENT_RUNBOOK.md — any hardcoded model references
- Any shell scripts calling `claude -p --model <old-id>`
- GitHub Actions workflows using claude-code actions

Jarvis currently uses the default Opus 4.8 (set by CC v2.1.154 as the new default), so there should be no hardcoded old model IDs. Verify with: `grep -r "claude-sonnet-4-20250514\|claude-opus-4-20250514" /home/user/jarvis-feed/`

### Action 3: Decide Overflow Behavior

If the $200 credit is exhausted mid-month, requests can either:
- **Fail silently** (default) — programmatic calls error, interactive use continues
- **Overflow at API rates** — you get billed at standard API prices for usage beyond $200

For Jarvis, estimated monthly usage: ~50K input + 10K output per run × 2 runs/day × 30 days ≈ **$28-35/month** at Opus 4.8 pricing. The $200 credit is more than enough. No overflow risk under normal operation.

**Recommendation:** Enable overflow as a safety net (a $5 API overage is better than a missed digest), but don't expect to need it.

---

## Summary

The June 15 billing change is low-risk for Jarvis given the $200 Max 20x credit. The only real action items are:

1. **June 8:** Claim credit via Anthropic email
2. **Before June 13:** Run the grep audit above to confirm no old model IDs
3. **Optional:** Enable overflow protection in Claude account settings

The community consensus (HN thread #48130374) is that the $200 credit is generous for lightweight scheduled automation — Jarvis is well within bounds.
