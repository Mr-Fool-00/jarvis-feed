# Cole Medin's second-brain-skills — 7/10

## What it is
Cole Medin (AI educator with hundreds of thousands of followers) spent 2 months building a "second brain" using Claude Code + skills — a system that runs automatically all day and handles content creation, presentation building, video generation, and organizing his Obsidian vault. He then open-sourced the whole thing. It's a collection of 22 skill files you drop into Claude Code that turn it from a coding tool into a full-time knowledge work assistant.

## Why you'd want it (specific to your stack)
You're building Jarvis to do exactly this — run autonomously, remember context across sessions, and feed your book + content pipeline. Cole's system has three things that map directly to what you need:
1. **Content engine that runs 24/7** — his skills generate YouTube scripts, social posts, PPTs automatically. Your Jarvis pipeline produces books. Same idea, different output.
2. **Obsidian vault integration** — he uses SOUL.md, USER.md, MEMORY.md as Claude's persistent memory across sessions. This is the memory architecture your Jarvis system needs to actually remember who you are.
3. **Progressive Disclosure of Context** — his key design insight: don't dump all context in every session. Each skill loads only what it needs. This is how you keep Claude fast and accurate as your skill library grows past 30+ skills.

He says the system saves him "at least a dozen hours of work weekly."

## Why I think it's worth your attention
This is the clearest "someone did the work for you, now steal the design" item I've seen in weeks. The 24/7 automation + Obsidian memory + progressive disclosure combination is exactly the architecture you should be building this summer. Not installing it — building your own version from this blueprint.

## What to do (safety rule — third-party code)
I won't install this directly. Per the safety gate, here's the plan:
1. You review this briefing and say "yes, this looks like the right direction"
2. I deep-read the specific skills you want (especially the SOUL.md/USER.md memory architecture and the content engine skill)
3. We build native Jarvis versions of the patterns we want — Jarvis-owned code, not Cole's code running on your system
4. The MCP client skill (connects to Zapier/GitHub) needs particular scrutiny before we build anything similar

React with 🚀 in Slack if you want me to start the deep-dive + build. React with 👍 to acknowledge and defer to after finals. React with 👎 to skip.

🔗 https://github.com/coleam00/second-brain-skills
🔗 https://github.com/coleam00/second-brain-starter (PRD generator for building your own second brain)
