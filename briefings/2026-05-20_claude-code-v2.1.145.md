# Claude Code v2.1.145 — 8/10

## What it is
Today's Claude Code update (May 19-20, 2026) ships three small but genuinely useful things: you can now list all your live Claude sessions as structured JSON, multi-agent tracing now shows which agent spawned which, and the plugin browser shows you exactly what a plugin installs before you say yes.

## Why you'd want it (specific to your stack)
The `claude agents --json` command is the one you've been missing for the Jarvis session dashboard. Right now you can see running agents in the Agent View GUI — but you can't script against that list. Now you can: write a shell one-liner that shows all running book-pipeline agents, their durations, and which parent dispatched them. Pair it with the OTEL trace fix and your multi-chapter parallel runs will finally produce readable trace graphs in your terminal. The plugin preview is just quality of life — no more guessing what a plugin touches before you install.

## Why I think it's worth your attention
Small release, but `claude agents --json` specifically is the "scriptable session management" missing piece for Jarvis's ops layer. Worth 20 minutes to wire it into your shell prompt.

## What to do
Update via `npm install -g @anthropic-ai/claude-code@latest`. Try `claude agents --json | jq '.[].name'` to list active session names. That's it to start — build from there.

🔗 https://claudelog.com/claude-code-changelog/
🔗 https://code.claude.com/docs/en/changelog
