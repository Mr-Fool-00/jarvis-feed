# Claude Code Channels — 9/10

## What it is
Claude Code Channels is a feature that lets any external service (Slack, Telegram, Discord, your phone, a CI system) push messages directly INTO a running Claude Code session. Think of it like a hotline between the outside world and Claude while it's working. Claude reads the incoming message, acts on it, and can reply back through the same channel.

Anthropic ships it with official plugins for Telegram, Discord, and iMessage. The community has already built Slack adapters. It runs on the MCP protocol, so custom channel plugins are straightforward to build.

## Why you'd want it (specific to your stack)
Right now Jarvis routes Slack notifications by committing files with special prefixes like `notify:`, `digest:`, `briefing:` and letting GitHub Actions translate those commits into Slack messages. That's a workaround — it works, but it adds latency, requires GitHub to be healthy, and only goes ONE way (Jarvis → you).

With Channels, a running Jarvis CCR session could have a live Slack channel attached. When you react with 🔥 to a digest item in Slack, that reaction pushes into the session immediately — no commit roundtrip. Your P4 nag system (the Slack DM calendar nudges you wanted) becomes a two-line config addition instead of a separate infrastructure build.

This also unlocks the read direction: Jarvis could receive your @mentions from Slack while running a research task in the background. You'd tap the shoulder, it would respond.

## Why I think it's worth your attention
This is the architectural fix for the single biggest current limitation in Jarvis: the notification system is one-directional and has a 30-second+ latency because it routes through git commits. Channels makes it real-time and bidirectional. That's the difference between a pager and a phone.

## What to do
The Channels docs are live at `code.claude.com/docs/en/channels`. No waiting list, no preview sign-up — it works with any Claude Code session as of v2.1.80+. Two specific next steps:

1. **Read the docs** — 10 minutes. Understand the channel protocol and the `--channels` flag.
2. **Decide: build a native Jarvis Slack channel, or use retrodigio's community adapter as a reference.** The community `claude-channel-slack` repo shows the implementation pattern. Per the safety rule: I won't install theirs — but I can build ours using their pattern as a guide when you say go.

🔗 https://code.claude.com/docs/en/channels
🔗 https://github.com/retrodigio/claude-channel-slack (reference implementation, do not install)
