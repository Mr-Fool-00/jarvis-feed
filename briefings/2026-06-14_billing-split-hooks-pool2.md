# June 15 Billing Split: Your Hooks Move to a Paid Pool Tomorrow — 9/10

## What it is

Tomorrow (June 15) Anthropic splits Claude billing into two separate buckets. The part you already knew: programmatic AI calls (like scripts using `claude -p`) get their own credit pool. The part that's less obvious and more relevant: **Claude Code hooks — the `PreToolUse`, `PostToolUse`, `Stop`, and `UserPromptSubmit` hooks you configure in `settings.json` — are classified as programmatic after June 15.** That means every hook that fires in a local Claude Code session draws from the new paid credit pool, not your flat $200/month Max plan.

## Why you'd want to know this (specific to your setup)

Your fiction pipeline and Jarvis both run hooks. If those hooks run in your **local** CC sessions (the ones you start with `claude` in your terminal), they're affected. There's good news for Jarvis specifically: CCR (Claude Code for web, which is what these routines run on) is explicitly exempt — Jarvis routines stay on Pool 1 subscription limits. But any hooks you've set up in your local `~/.claude/settings.json` for your daily fiction pipeline work will hit Pool 2 starting tomorrow morning.

Also on June 15: two old model IDs retire. If any of your automation references `claude-3-5-sonnet-20241022` or other legacy model ID strings, those error out tomorrow.

## Why I think it's worth your attention

This is the most direct operational impact to your setup in the last week — it's happening in less than 24 hours, and missing it means waking up to billing surprises in your credit pool.

## What to do

1. Open `~/.claude/settings.json` right now and look at your hooks section. If you don't have hooks configured there, you're fine for tomorrow.
2. If you do have hooks: read the hook-heavy operator guide (link below) to understand which get billed to Pool 2 and whether you'll exhaust your credit budget.
3. Stopgap if needed: a ~50-line Python wrapper replicates `claude -p` one-shot behavior using an interactive session (which stays on Pool 1). The same gist has the hook audit template.
4. CCR routines (like Jarvis) don't need changes — they're exempt.

🔗 https://gist.github.com/yurukusa/7d854616809e673ca8d23353ed8267a6
🔗 https://news.ycombinator.com/item?id=48210010
