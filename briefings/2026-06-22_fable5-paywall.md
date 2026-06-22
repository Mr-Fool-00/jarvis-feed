# Fable 5 Leaves Your Max Plan Tonight — 9/10

## What it is

Fable 5 — Anthropic's best publicly released model — has been free to use on your $200/mo Max plan since it launched June 9. Tonight at midnight that ends. Starting June 23, using Fable 5 draws from a separate credit pool at full API rates: $10 per million tokens in, $50 per million tokens out. That's double what Opus costs. Anthropic's making the change to separate "flat subscription" from "usage-based API consumption" — the model is too expensive for them to cover in a flat plan indefinitely.

## Why you'd want to know about this (your stack specifically)

Your book pipeline runs long autonomous sessions — planning passes, chapter drafts, fixer loops. If Fable 5 sneaked into your default model setting (the free window made it tempting), a single long chapter run could now cost you $10-20+ in credits. On a $200/mo plan where most of that covers Opus and Sonnet usage, that adds up fast. This is a "check your settings tonight" moment, not a "read about it later" moment.

What to do right now: check what `claude --model` returns in your pipeline config. If it's `claude-fable-5`, swap it to `claude-opus-4-8` for your writing agents and `claude-sonnet-4-6` for your fast loops.

## Why I think it's worth your attention

This is the single biggest change to your cost model in the last month. You've been building book pipeline throughput assuming a certain cost baseline. That baseline just changed at midnight.

## What to do

1. Check your CLAUDE.md and any agent configs for model overrides
2. Set Opus 4.8 as default for long autonomous runs
3. Use Sonnet 4.6 for fast/iterative loops (it's significantly cheaper and handles most writing tasks well)
4. If you want to keep using Fable, do the math: at $50/M output tokens, a 100K-word chapter is ~$25+ in output costs alone

🔗 https://claudefa.st/blog/guide/development/fable-5-usage-credits
🔗 https://www.mindstudio.ai/blog/claude-fable-5-pricing-access-usage-limits
