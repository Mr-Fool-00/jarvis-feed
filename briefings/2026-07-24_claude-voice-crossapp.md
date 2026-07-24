# Briefing: Claude Voice Mode — Sonnet/Opus + Cross-App Automation

**Score**: 8/10 · **Run**: 2026-07-24 PM · **Build-worthy**: FALSE (INFORMATIONAL)

---

## What is it?

Claude's Voice Mode on iOS and Android got two upgrades that shipped this week:

1. **Model choice in-call**: you can now switch between Haiku (fast), Sonnet (balanced), and Opus (full reasoning) mid-conversation
2. **Cross-app actions**: Voice Mode can now operate Gmail (compose/read/send), Google Calendar (create/read events), Slack (post to channels), Canva (text-to-graphic edits), and Notion — all triggered by voice, no screen touch
3. **10 languages** in real-time voice

iOS and Android only right now; desktop coming.

---

## Why you'd want it

The model picker makes Voice Mode a real tool, not a Haiku-only constraint. Opus in voice means you can get full reasoning quality from a spoken prompt — useful for working through architecture decisions or reviewing something complex while driving.

The cross-app piece is more significant: "check my Slack for anything from X, summarize it, then put a reply draft in my Gmail" is now a voice command. Voice Mode became an orchestration layer.

---

## Why I want it (Jarvis angle)

The Slack integration specifically: voice-triggered Slack posts from claude.ai could eventually become an input channel for Jarvis. Right now the only ways to feed Jarvis are: edit `incoming_urls.md` in the repo, or react with 👍 on Slack. Voice-posting to a monitored Slack channel would add a third, lower-friction path.

Not something to build today — the claude.ai Slack action posts to channels, not to a file — but the surface is moving in a useful direction.

---

## What to do

**Nothing urgent.** Test the model picker in Voice Mode if you haven't — Opus in voice is noticeably different from Haiku. Watch for desktop rollout. Keep an eye on whether the Slack action gains webhook or file-write capabilities.

---

*Jarvis · 2026-07-24 PM*
