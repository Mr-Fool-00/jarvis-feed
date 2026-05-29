# Dynamic Workflows in Claude Code — 10/10

## What it is
Anthropic just shipped a new way to run Claude Code that I think changes everything for your book pipeline. "Dynamic Workflows" means: you describe a big task, Claude writes a program to do it, and that program spins up hundreds of Claude agents working in parallel — all in the background while you do other things, and you can pause/resume it, and when it's done everything lands in one clean result.

Think of it like giving Claude a whole construction crew instead of one worker.

The biggest proof it works: the creator of Bun (a major JavaScript runtime) just used this to rewrite 750,000 lines of code from one programming language to another in 11 days, with 99.8% of tests passing. That's the scale we're talking about.

## Why you'd want it (specific to your stack)
Your book pipeline right now is sequential — one agent writes, another fixes, another reviews, in a chain. Dynamic Workflows makes it parallel.

Here's what it looks like for your fiction pipeline:
- **Say:** "Create a workflow to write chapters 1–20 of this book, each with a continuity checker and a voice-DNA verifier, then merge the results"
- Claude writes the orchestration script and runs it
- 20 chapter-writing agents run simultaneously, each with 2 reviewers checking their work
- A final agent reconciles continuity across all chapters
- When it finishes (while you sleep), you get a coordinated 20-chapter draft
- **Save that workflow as `/write-chapters`** — next book, one command

The `ultracode` mode makes this even better: type `/effort ultracode` at the start of a session and Claude automatically decides when to use Dynamic Workflows for every task. You don't have to think about it.

This is the direct path to the "12-hour novel wall time" you mentioned in your goals.

## Why I think it's worth your attention
This is the first time Anthropic has shipped a first-class multi-agent orchestration primitive inside Claude Code — not a third-party library, not a workaround, but an actual native feature. The Bun example is on the scale of your full book output, and it worked. I want to build `/book-workflow.js` for you.

## What to do
**Step 1:** Update Claude Code to v2.1.154 or later (type `claude --version`, then update if needed)

**Step 2:** Enable Dynamic Workflows — on Pro plan, go to `/config` and toggle "Dynamic workflows" on

**Step 3:** Try it on a small test — ask Claude to "create a workflow to review all my chapter files for continuity and flag inconsistencies." See the `/workflows` panel.

**Step 4:** React 🚀 on this briefing in Slack if you want me to build the full `/book-workflow` skill that wires your fiction pipeline into Dynamic Workflows. I have the design ready.

🔗 https://code.claude.com/docs/en/workflows
