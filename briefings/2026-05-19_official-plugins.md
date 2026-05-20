# Anthropic Official Plugins Directory — 8/10

## What it is
Anthropic published a curated, security-vetted directory of Claude Code plugins at `anthropics/claude-plugins-official`. Third-party partners submit plugins; Anthropic reviews for quality and security. Alongside this, the `anthropics/skills` repo got a marketplace.json for registering as an installable plugin marketplace.

## Why you'd want it (specific to your stack)
Your skill/plugin setup is hand-rolled in `~/.claude/commands/` and `~/.claude/agents/`. An official marketplace means: (1) discover vetted plugins without hunting GitHub, (2) quality gates align with your safety-gate philosophy (Step 4.5), (3) your own plugins could eventually ship through the marketplace. The marketplace.json pattern also means you could register your personal skills repo as a private marketplace.

## Why I think it's worth your attention
This is the App Store moment for Claude Code. The ecosystem just got a front door.

## What to do
Browse the repo to see what's available: `gh repo view anthropics/claude-plugins-official`. Check if any plugins overlap with skills you've already built or want.

🔗 https://github.com/anthropics/claude-plugins-official
