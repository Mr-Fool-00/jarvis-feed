# Claude Code Post-Leak Insights (curated list) — 7/10

## What it is
On March 31, 2026, someone at Anthropic forgot to add `*.map` to `.npmignore` and accidentally shipped 512,000+ lines of Claude Code's TypeScript source. A GitHub repo has since curated the best analyses of what was found. Key discoveries: KAIROS (a background "proactive assistant" agent that was hidden), a 3-layer context compression pipeline (MicroCompact → AutoCompact → Full Compact), Undercover Mode (90 lines that strip Anthropic's identity from open-source contributions), fake tool injection for anti-distillation, and regex-based frustration detection ("wtf", "this sucks", etc.).

## Why you'd want it (specific to your stack)
Understanding how Anthropic actually built Claude Code's internal agent loop informs how you build Jarvis. The 3-layer compression design specifically is useful: knowing when and how Claude Code compacts context helps you design Jarvis skills that don't fight the compressor. The KAIROS pattern (a background agent that proactively surfaces suggestions) is basically the blueprint for what you'd want the Jarvis discovery loop to eventually become — a persistent background presence, not a 12-hour cron.

## Why I think it's worth your attention
This is the closest thing to official Claude Code architecture documentation that exists. Reading it is 2 hours that'll pay off in better Jarvis design decisions.

## What to do
Read the curated list. No installation, no action — pure reading. Start with the Architecture section (context compression), then Undercover Mode (interesting for how you think about Jarvis's identity in commits).

🔗 https://github.com/nblintao/awesome-claude-code-postleak-insights
🔗 https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/
