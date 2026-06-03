# Claude Code v2.1.157: Local Plugin Auto-Load — 8/10

## What it is

As of May 29, Claude Code automatically loads any plugin you drop into your local `.claude/skills/` folder — no publishing to a marketplace, no install command, nothing. You write a skill, put it there, restart CC, and it's live. There's also a new command: `claude plugin init <name>` scaffolds the full plugin folder with the right template files so you don't have to copy-paste structure every time.

## Why you'd want it (specific to your stack)

Your current skill development loop has friction: write skill → figure out how to install → test. That middle step just went away. Every skill you've built or will build lives in `.claude/skills/` now and loads automatically. For your fiction writing pipeline, your Council skill, your Jarvis system — you can iterate on all of them without any packaging step. The `plugin init` command means starting a new skill takes 10 seconds instead of however long template-hunting took before.

There's also a smaller-but-useful fix in the same release: the `agent` field in `settings.json` is now honored when you dispatch subagents via `claude agents`. That means your per-project agent configuration (if you've set default agents per project) actually applies to your dispatched background sessions now, not just the main session.

## Why I think it's worth your attention

This is the kind of quiet changelog item that changes your day-to-day more than a model launch does. You're actively building skills. The friction between "wrote it" and "using it" just dropped to zero.

## What to do

Update Claude Code if you haven't since May 29 (`claude --version` should show 2.1.157 or later). Check what's in `~/.claude/skills/` or your project's `.claude/skills/` — anything there should now load on startup. If nothing is there yet, run `claude plugin init my-first-skill` to see the scaffolded template.

🔗 https://code.claude.com/docs/en/changelog
