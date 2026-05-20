# CI Auto-fix + Remote Control — 8/10

## What it is
Two new Claude Code features that just shipped at Code w/ Claude London (May 19-20):

**CI Auto-fix**: Claude watches your pull requests in Anthropic's cloud. When CI fails, it reads the error, figures out the fix, writes it, and pushes it to your branch with a plain-english explanation. When a reviewer leaves a comment, Claude makes the change and replies to the thread — labeled "Claude Code" under your username. No more "CI red → go fix it" loop.

**Remote Control**: You can start a session on your Mac, check it from your phone, and pull it back to your terminal with `claude --teleport`. Sessions aren't stuck to one machine anymore.

## Why you'd want it (specific to your stack)
Your book pipeline generates a PR per chapter batch for review. Right now, any CI failure (lint error, test flake, formatting issue) kills the batch and needs manual intervention. CI auto-fix closes that loop — the pipeline keeps moving while you sleep. The remote control piece is specifically useful for late-night kickoffs: start a long chapter run from your Mac, check progress from your phone, grab it back in your terminal when you're ready to review the output. Both features are available on Max plan.

## Why I think it's worth your attention
CI auto-fix is one of those features that sounds convenient until you realize it means your pipeline can run 8 hours unattended and actually finish. That's a real quality-of-life change for overnight book runs.

## What to do
CI auto-fix: run `/autofix-pr` inside a Claude Code session in your repo — Claude will set up the cloud watcher. Remote control: update to v2.1.145 and try `claude --teleport` from a second machine when you have an active session. Both are Max-plan included.

🔗 https://www.mindstudio.ai/blog/code-with-claude-2026-new-agent-features
🔗 https://paddo.dev/blog/claude-code-auto-fix-pr-lifecycle/
