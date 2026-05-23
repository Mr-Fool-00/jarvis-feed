# Skillgrade — Unit Tests for Your Agent Skills — 7/10

## What it is
Skillgrade is a testing tool that runs your Claude skill twice — once with the skill loaded, once without — and has an AI judge compare the results. You get a pass/fail report showing whether your skill actually makes Claude better at the task, or is just dead weight. Think of it like running unit tests on code, but for your SKILL.md instruction files.

## Why you'd want it (specific to your stack)
You're building a growing library of Jarvis skills — for writing, for research, for orchestration. Right now there's no way to know if a skill you wrote actually helps, or if Claude performs just as well without it. Skillgrade would let you write a few test tasks ("write a chapter intro," "summarize this world-bible entry"), run them, and get a concrete score on whether each skill earns its place in context. Directly relevant to your writing pipeline and Jarvis skill library.

## Why I think it's worth your attention
This closes the one gap that makes skill development feel like guesswork. Right now you write a skill, vibe-test it, and ship it. With this, you'd have a number. That matters when you're building 15+ skills over a summer.

## What to do
The tool requires an ANTHROPIC_API_KEY (not Max plan) so you can't use it directly. But the concept is simple enough that I can build a native version for you that runs inside Claude Code Max — no API keys needed. React 🚀 if you want me to queue that build.

🔗 https://github.com/mgechev/skillgrade
🔗 Blog post: https://blog.mgechev.com/2026/03/14/skillgrade/

---
*Safety gate: this is third-party code. Will not install directly. If approved, I'll build a native version for your stack.*
