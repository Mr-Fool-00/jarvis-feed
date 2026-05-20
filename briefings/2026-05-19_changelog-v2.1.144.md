# Claude Code v2.1.144 changelog — 7/10

## What it is
Latest Claude Code release (May 19). Key changes: `/resume` now works for background sessions, startup no longer hangs up to 75s when api.anthropic.com is unreachable (15s timeout), `/model` changes current session only (press `d` to set default), "extra usage" renamed to "usage credits". v2.1.143 added plugin dependency enforcement and context cost estimates in marketplace. v2.1.142 added `claude agents` config flags.

## Why you'd want it (specific to your stack)
The startup hang fix is real quality-of-life — if your VPN or network hiccups, Claude Code was hanging for over a minute. Background session `/resume` means you can pick up where Agent View sessions left off. Plugin dependency enforcement means the plugin ecosystem is getting more reliable.

## Why I think it's worth your attention
Incremental but solid. The startup fix alone is worth the update.

## What to do
Run `claude update` to get v2.1.144.

🔗 https://releasebot.io/updates/anthropic/claude-code

<!-- resurfaced 2026-05-19 with enriched #ai-news format -->
