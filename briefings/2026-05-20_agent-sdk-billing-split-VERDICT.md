# VERDICT: agent-sdk-billing-split — SKIPPED

**Score:** 9/10 pre-deep-dive (importance), N/A post (not buildable)
**Decision:** SKIP
**Re-reviewed:** 2026-05-20

## Reason
Agent SDK billing separation (June 15). OPERATIONAL WARNING category — critically important for Jarvis planning but not a buildable skill. A `/billing-forecast` command can't be built because (1) the billing split hasn't gone live yet, (2) the CLI doesn't expose credit usage queries, and (3) token counting per `claude -p` invocation requires API-level instrumentation Leo doesn't have. Revisit when the split is live and credit APIs exist. For now: awareness only.
