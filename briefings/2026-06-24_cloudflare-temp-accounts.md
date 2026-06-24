# Cloudflare Temporary Accounts for AI Agents

**Score:** 7/10 · **Published:** June 19, 2026 (Cloudflare Agents Week)  
**Source:** blog.cloudflare.com (Agents Week) + simonwillison.net (June 21, 2026)

---

## What it is

Cloudflare shipped a first-class primitive for giving AI agents disposable cloud environments: `wrangler deploy --temporary`.

One command spins up a temporary Cloudflare account with a **60-minute auto-expiry**. The agent can use Workers, KV, D1, and Durable Objects inside that account — then the account self-destructs. No cleanup step, no credential rotation needed afterward, no permanent credential held by the agent.

Simon Willison explored the pattern on June 21 in the context of his datasette-agent work, where agents publish or fetch live data without holding permanent secrets.

---

## Why you'd want it

The security problem with agentic integrations is permanent credentials. Once you wire an agent to a service (Cloudflare, GitHub, Slack), it holds those credentials indefinitely. If the agent misbehaves, runs a rogue task, or gets compromised, that credential is still live.

Temporary accounts solve this by design: the credential only exists for the duration of the task. After 60 minutes, the account is gone. Nothing to rotate, nothing to clean up.

---

## Why I think it matters for Leo

Jarvis currently holds PAT + Slack webhooks permanently in the routine prompt. That's the correct call for the current setup (Jarvis is stateful, ongoing, and ops-scoped). But as Jarvis expands — file hosting, social posting, ad-hoc Workers deployments, anything that touches an external service — each new integration is another permanent credential added to the attack surface.

The `--temporary` pattern is the right hygiene model for any future Jarvis expansion that:
- Needs a real cloud environment (not just an API call)
- Is task-scoped (spin up, do work, expire)
- Shouldn't persist beyond the run

Worth reading before wiring up the next integration. The Cloudflare Agents Week post has the full implementation details; Willison's write-up has the pattern abstracted for non-Cloudflare use.

---

## What to do

1. **Read:** blog.cloudflare.com (Cloudflare Agents Week, June 19) — `wrangler deploy --temporary` docs
2. **Read:** simonwillison.net/2026/Jun/21/temporary-cloudflare-accounts — pattern abstracted from Cloudflare specifics
3. **Before the next new integration:** ask "should this be temporary?" If the answer is yes, use `wrangler deploy --temporary` instead of a permanent account

No code to build today. This is a pattern to internalize now so you use it correctly the next time an integration comes up.
