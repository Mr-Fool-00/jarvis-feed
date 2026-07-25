# Claude Voice Mode: Opus + Cross-App Automation (Gmail/Calendar/Slack) — 9/10

**Date:** 2026-07-25
**Source URL:** https://claude.com/blog/think-through-hard-problems-in-voice-mode
**Score:** 9/10
**Category:** Anthropic product update — affects Jarvis P3 architecture

---

## What it is

Anthropic just unlocked voice mode for all three model tiers — Opus, Sonnet, and Haiku — for paid users. It was Haiku-only before. At the same time, they shipped cross-app voice automation: speak a command, it triggers real actions in Gmail, Google Calendar, Slack, Canva, or Notion without switching apps. Free plan stays Haiku-only with one connected app. Pro/Max/Team gets everything. Ten languages. Voice usage counts against your regular plan limits — no separate bucket.

---

## Why you'd want it (specific to your stack)

Jarvis P3 was being designed as a lightweight Haiku voice interface — quick lookups, simple queries. That design is now obsolete.

Opus voice means P3 can be a full reasoning agent in voice form: complex planning, multi-step synthesis, the kind of analysis that used to need a text session. And the five cross-app integrations — Gmail, Google Calendar, Slack especially — are exactly the "morning brief" tools you described wanting. One voice turn: "What's on my calendar today? Any flagged Slack messages? What's in my inbox?" All answered, all from a single Opus-powered voice agent.

The concrete P3 revision: instead of "voice frontend that hands off to text for anything hard," P3 becomes "voice-first Opus agent with direct calendar/Gmail/Slack access." The Haiku fallback disappears. The architecture simplifies.

---

## Why I think it's worth your attention

This is the biggest shift in the voice roadmap since voice launched. Haiku voice was a toy — useful for "set a timer" style commands. Opus voice is the full system. The cross-app integrations are the practical unlock: real calendar reads, real email summaries, real Slack digests, all without opening a browser. That's the morning brief workflow you've been describing since the P3 spec was first drafted.

The timing is also good: Opus 5 is now the Max default (AM run today). So "Opus voice" in practice means Opus 5 voice — the same model Leo uses for everything else, now accessible by voice with action capabilities.

---

## What to do

The P3 spec needs a revision pass before building starts.

1. **Revision target:** Swap the architecture from "Haiku voice query interface" to "Opus 5 voice reasoning agent with Gmail + Calendar + Slack integration."
2. **Morning brief flow:** Design one voice command that pulls today's schedule, flags Slack messages, and surfaces top inbox items — all in one turn.
3. **Build gate:** After spec revision, P3 build starts. React 🚀 on the digest to kick it off.

🔗 https://claude.com/blog/think-through-hard-problems-in-voice-mode
