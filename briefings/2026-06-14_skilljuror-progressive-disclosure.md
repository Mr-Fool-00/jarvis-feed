# SkillJuror: Reorganize Your Skill Files to Get 3x More Usage Out of Them — 7/10

## What it is

A research paper (June 10, 2026) ran a controlled study comparing two ways of organizing AI agent skill files:

- **Flat**: everything in one big file (how most skill setups work today)
- **Progressive Disclosure**: a short root file that says "here's what I can do" with links to detailed docs that only load when needed

The result: agents with Progressive Disclosure structure used **3.3× more of their available skills** per task and completed **4.1% more tasks correctly**. Same skills, different organization.

## Why you'd want it (specific to your stack)

Right now your skills are almost certainly flat — each SKILL.md contains all its content inline. The research says this causes agents to load only 1-2 skills per run even when 10+ are relevant. A progressive disclosure structure has:

- A root index file (~50-100 tokens per skill, just the name, what it does, and how to invoke it)
- Separate detail files (`skills/voice-dna/README.md`, `skills/council/README.md`, etc.) loaded on demand

For your fiction pipeline: Claude currently may ignore your character-voice skill or continuity checker because the flat context is too heavy and it stops reading after the first few skills. With a progressive index, it actively pulls the relevant skill when the chapter needs it. This is the difference between skills that sit unused and skills that fire at the right moment.

## Why I think it's worth your attention

This is the first empirically validated answer to "how should I organize my SKILL.md files" — not a best guess, an actual controlled study with 82 tasks and 410 matched trials. The +3.3x uptake number is big. Your skill library is growing; this is the time to set it up right before it gets unwieldy.

## What to do

Two options:
- **A) Leo approves → I build the reorganized structure.** I'll read your current skill files, create a root index with metadata per skill (~50 tokens each), and move full content to per-skill directories. Leo reviews before anything changes.
- **B) DIY.** Create `~/.claude/skills/INDEX.md` with one line per skill (name, one-sentence description, invocation). Keep full skill content in individual files. Tell Claude "when starting a task, check INDEX.md first and load relevant skills as needed."

React 🚀 on this message if you want me to build Option A.

🔗 https://arxiv.org/abs/2606.11543
