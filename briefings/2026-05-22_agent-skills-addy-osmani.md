# addyosmani/agent-skills — 8/10

## What it is
Addy Osmani (Google Chrome engineering lead) built a GitHub project called "agent-skills" that packages the habits of a senior engineer into files Claude can actually read and follow. When you run a coding task, Claude automatically loads the relevant skill — so it writes specs before coding, runs tests before shipping, does security reviews before merging. Without it, Claude takes the shortest path to "done" and skips the good-engineering parts.

## Why you'd want it
Your Claude Code sessions already have CLAUDE.md telling Claude what to do, but it's one big file Claude has to read every time. This project uses a cleaner pattern: small, focused skill files that get loaded only when relevant (writing code → loads the build skill; reviewing → loads the code-review skill). The three specialist personas — code reviewer, test engineer, security auditor — map directly to your Council pattern where different agents have different jobs. You could build a version of this for your writing pipeline: a "story-arc" skill, a "voice-consistency" skill, a "chapter-beat" skill — each injected contextually instead of crammed into one giant prompt.

## Why I think it's worth your attention
Addy is one of the highest-signal sources in the Claude Code space and this is the pattern your CLAUDE.md system is already converging on. The repo crossed 44.8K stars with 193 commits and an MIT license — this is mature and real. I did a full safety review: no external network calls, no credential hooks, clean shell scripts, no red flags.

## What to do
DON'T install it directly (safety gate rule — third-party code needs Leo's approval first, then I build our own version inspired by it, not a copy).

React 🚀 to this briefing if you want me to design a Leo-native version for your writing pipeline. React 👍 to just note it. React 👎 to skip it.

🔗 https://github.com/addyosmani/agent-skills
🔗 Blog post: https://addyosmani.com/blog/agent-skills/

---
**Safety assessment (Step 4.5):**
- Tools requested: Claude Code slash commands, Cursor rules, Gemini CLI hooks
- Files installed: 23 skill .md files, 3 agent persona .md files, session lifecycle hooks
- Network calls: None detected in source
- Maintainership: 193 commits, 44.8K stars, 4.9K forks, MIT license, 42 open issues, active PRs
- Red flags: None found
- Post-deep-dive score: 8/10 (maintained — no downgrade warranted)
