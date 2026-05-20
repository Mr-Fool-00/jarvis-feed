# Claude Code Agent View — 9/10

## What it is
A new built-in dashboard for Claude Code (`claude agents`) that shows all your background sessions in one screen. Dispatch tasks, peek at progress, reply to questions, attach for full conversation — all from a single terminal view. Sessions run via a supervisor daemon that persists across sleep. Each session auto-isolates in a git worktree.

## Why you'd want it (specific to your stack)
You run parallel agents constantly — context-fetcher, slack-formatter, fixer chains, research tasks. Right now you manage these via tmux/tabs and the Agent tool, which means context-switching between windows to check status. Agent View replaces that with one dashboard: dispatch a fixer pass, a research task, and a code review as three rows. The worktree isolation means parallel sessions can't stomp each other. The `/goal` command lets you set completion conditions so sessions know when they're done.

## Why I think it's worth your attention
This is the single biggest quality-of-life upgrade for multi-agent Claude Code users since subagents shipped. It's exactly the workflow pattern your Jarvis thinker (Max) would benefit from.

## What to do
Check your Claude Code version (`claude --version`). If you're on v2.1.139+, try `claude agents` right now. Dispatch a simple task ("investigate the jarvis-feed repo structure") and watch the dashboard work. Key shortcuts: Space to peek, Enter to attach, left-arrow to detach.

🔗 https://code.claude.com/docs/en/agent-view
