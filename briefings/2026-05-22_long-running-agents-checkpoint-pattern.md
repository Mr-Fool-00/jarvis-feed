# Long-running Agents — Checkpoint + Handoff Pattern — 8/10

## What it is

A blog post by Addy Osmani (Google Chrome Engineering Director, one of the most-read people on Claude Code) laying out the 5 patterns that separate working long-running agents from weekend demos.

The one pattern you don't have: the **Handoff File**. At the end of every session, the agent writes one compact file that contains everything the NEXT session needs to pick up exactly where it left off. Not a chatty summary — a structured state file. The next session boots, reads the file, and continues like nothing stopped.

Companion to Anthropic's own `cwc-long-running-agents` harness patterns from the London keynote last week.

## Why you'd want it

Your novel-writing pipeline runs across multiple Claude sessions. Right now, each chapter session starts cold — it re-reads whatever context you've set up, but it doesn't know what the LAST session was mid-doing, what voice-DNA calibrations it made, or what plot threads it left open.

A `/checkpoint` skill that writes `handoff.md` at session end + reads it at session start would make your writing agent sessions feel continuous. Chapter 7 would know what Chapter 6 decided and why. That's the jump.

Same thing for multi-step builds with Jarvis — if a session gets interrupted mid-task, today it restarts from scratch. With a handoff file, it restarts from where it stopped.

## Why I think it matters

The other four patterns (checkpoint to disk, separate evaluation, context reset, append-only log) I can also implement for you — they're all buildable as lightweight skills or hooks. Together they'd make your agent work significantly more reliable across long projects.

The bigger picture: Addy's framing is "harness design is the skill most underestimated by people who think prompting is the hard part." That's the right frame. Your harness is good. These patterns would make it great.

## What to do

React 🚀 on the #improvements post → I'll build a `/checkpoint` skill that implements the handoff file pattern. No third-party installs. Pure CLAUDE.md + commands/ code that we own and can tune.

Estimated implementation: one session, produces `~/.claude/commands/checkpoint.md` + a project-level hook that reads `handoff.md` on session start.

🔗 https://addyosmani.com/blog/long-running-agents/
🔗 Substack mirror: https://addyo.substack.com/p/long-running-agents
