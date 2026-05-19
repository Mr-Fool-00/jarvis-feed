# Pipeline Status — 2026-05-19

Snapshot of what's working end-to-end vs what's still blocked.

## ✅ Working

| Component | Status | Validation |
|---|---|---|
| Anthropic CCR scheduled cron | working | Fired 7:10 AM CDT (silent fail, since fixed), then 10:23 AM CDT (full delivery) |
| Discovery via WebSearch + WebFetch | working | 28 queries / 25 items / 15 surfaced in 10:23 AM run |
| Persona + multi-pass ranking | working | Top 3 scored 9-10/10 with project-tied "why it matters" |
| Digest write to repo | working | `digests/2026-05-19_PM.md` committed by agent |
| GITHUB_PAT auth push | working | `jarvis-feed-agent` commits visible at github.com/Mr-Fool-00/jarvis-feed |
| State file maintenance | working | `state/seen.json`, `state/agent_suggestions.md`, `state/failures.log` all updated |
| GitHub → Slack bridge (commit notifications) | working | initial setup via /github subscribe |
| GitHub Actions Slack router (per-channel by prefix) | live as of 2026-05-19 12:51 PM CDT | replaces the broad /github subscribe; routes by commit prefix to #ai-news / #improvements / #wins / #errors |

## ❌ Blocked

| Component | Status | Workaround |
|---|---|---|
| Direct Slack webhook from CCR | blocked permanently | hooks.slack.com not in sandbox network allowlist; pivoted to GitHub→Slack bridge |
| Gmail draft auto-send | not exposed | Gmail MCP only has create_draft; manual send required, or pivot away from Gmail entirely |
| Gmail draft visibility | unclear | Drafts appear in neither grau.enterprises nor leo.p.grau accounts despite MCP returning success IDs; pending investigation |

## 🟡 Deferred (post-finals)

- CF Workers Slack listener (two-way chat from phone)
- P3 voice frontend (Mac SwiftUI app + hold-to-talk)
- P4 calendar-aware nag system
- /dump file-routing audit (already shipped command, needs project-specific tuning)
- Pipeline cost-audit implementation (skip-if-empty, cache-reasoning)

## Architecture summary

```
[Anthropic CCR cron, every 12h]
        ↓
  WebSearch + WebFetch discovery (no direct API curl)
        ↓
  Multi-pass ranking against INTEREST_PROFILE + feedback.md
        ↓
  Write digest + state files
        ↓
  git push via GITHUB_PAT (rewrites remote URL with x-access-token)
        ↓
  GitHub → Slack integration auto-posts to #ai-news (commit notification with link)
```

Leo opens the GitHub link in Slack notification → reads digest content on GitHub. Push-to-phone reliability via Slack mobile, content via GitHub. Best architecture given sandbox constraints.
