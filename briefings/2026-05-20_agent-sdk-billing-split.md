# Agent SDK Billing Split (June 15) — 9/10

## What it is
Starting June 15, Anthropic separates all programmatic Claude usage into its own monthly credit pool. Interactive use (web chat, desktop, terminal Claude Code, Cowork) stays on your normal subscription. But anything programmatic — Agent SDK calls, `claude -p`, Claude Code GitHub Actions, third-party agents like OpenClaw — now draws from a fixed credit bucket billed at API rates. Max 20x gets $200/month. Credits don't roll over.

## Why you'd want it (specific to your stack)
Your Jarvis daemon runs `claude -p` to spawn Max sessions for every inbox task. Under the new billing, each daemon invocation eats from the $200 credit pool instead of the general Max quota. At Opus API rates, $200 buys roughly 25-40 full sessions per month. Your discovery loop alone runs 2x/day = 60/month, plus regular Jarvis tasks. You need to audit per-run token costs and decide: budget carefully, optimize to Haiku for light tasks, or restructure Jarvis to use interactive sessions instead of `claude -p`.

## Why I think it's worth your attention
This is the single most consequential billing change since Max launched. 26 days until it hits. If you don't audit and plan, Jarvis could burn through its monthly credit pool in the first week.

## What to do
Read the full breakdown, then audit Jarvis's `claude -p` token usage per invocation. I can help run that audit whenever you're ready.

🔗 https://devtoolpicks.com/blog/anthropic-splits-claude-subscriptions-agent-sdk-credit-june-2026
