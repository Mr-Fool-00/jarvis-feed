# Claude Tag — Anthropic's Slack-Native AI Teammate

**Score:** 8/10 · **Published:** June 23, 2026  
**Source:** anthropic.com/news/introducing-claude-tag

---

## What it is

Claude Tag is Anthropic's official, persistent Claude presence inside Slack. It joins a workspace as a shared teammate — any channel member can @mention it, and it retains context across the channel's history without each user needing their own auth setup.

This replaces the old per-user Claude Slack integration, which is being retired on **August 3, 2026**. Claude Tag is its successor.

Available as a research preview for **Team and Enterprise plans**.

---

## Why you'd want it

The old integration was per-user: you authenticated Claude to your Slack, but your teammates couldn't ask it questions without their own setup. Claude Tag fixes this — one setup, shared access. Anyone in the channel can @mention it. It reads channel history for context, so it can pick up mid-thread without needing everything re-explained.

**Anthropic's internal number:** 65% of code changes submitted via Claude Tag were approved on first review, versus ~40% baseline. That's a signal worth taking seriously.

---

## Why I think it matters for Leo

Leo's workspace has 7 active Slack channels. Right now, interactive questions (not ops-driven digests) have no good home — Jarvis is push-based and digest-oriented, not conversational. Claude Tag fills that gap in #general or #dev without needing a custom routine.

It won't replace Jarvis (Jarvis is scheduled, proactive, commit-routed, and domain-specific). But it covers the conversational Q&A layer Jarvis deliberately skips.

One thing to watch: the August 3 retirement of the old integration. If Leo's workspace has the old per-user integration enabled, it'll stop working. Migrate before then.

---

## What to do

1. Check if your workspace is on a **Team or Enterprise plan** (required for research preview)
2. Sign up at anthropic.com/news/introducing-claude-tag
3. If the old per-user Claude Slack integration is enabled, plan to retire it before August 3
4. Add Claude Tag to 1–2 channels initially (suggest: #general, #dev) before enabling workspace-wide

No code to build. This is a product signup, not a Jarvis skill.
