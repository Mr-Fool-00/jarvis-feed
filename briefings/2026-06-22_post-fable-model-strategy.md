# Post-Fable Model Strategy: When to Use Opus, Sonnet, Haiku — 7/10

## What it is

With Fable 5 going behind a paywall tonight, two guides dropped this week about which Claude model to use for what. The core framework: match the task to the cheapest model that reliably gets it right. Don't use Opus where Sonnet works. Don't use Sonnet where Haiku works. Only escalate when you've confirmed the cheaper tier fails.

The routing ladder they recommend:
- **Opus 4.8** — Long autonomous runs, complex agent sessions, multi-step reasoning that takes hours, anything where errors are genuinely costly. This is your new Fable fallback.
- **Sonnet 4.6** — Daily coding, chapter drafting, code review, tool use, interactive back-and-forth. The sweet spot of capability-to-cost. Most of your work probably belongs here.
- **Haiku 4.5** — Bulk extraction, classification, fast checks, high-volume summarization where cost is the constraint.
- **GPT-5.5** — Cross-vendor fallback when Anthropic throttles or suspends models. Worth keeping tested.

## Why you'd want it (your stack specifically)

Your writing pipeline has multiple stages: outline planning, chapter drafting, continuity review, fixer pass, quality gate. Right now you might be using the same model for all of them. A routing layer that sends planning to Opus (needs deep reasoning), drafting to Sonnet (good enough + 3× cheaper), and consistency checks to Sonnet or even Haiku could cut your per-book credit cost by 40-60%.

The timing is perfect — Fable going paid tonight makes explicit model routing in your pipeline worth doing RIGHT NOW, not "eventually."

## Why I think it's worth your attention

You've been running these pipelines without explicit model routing because unlimited Fable was there. That era ends tonight. Routing isn't optional anymore if you want to stay within the $200/mo budget for 10-15 books per summer.

## What to do

1. Map your pipeline stages to model tiers (I can help with this — just tell me your current stage breakdown)
2. Add an explicit `model:` field to each agent definition in your book pipeline
3. Run one book end-to-end with the new routing and compare quality vs. credit spend vs. Opus-everywhere
4. Set Sonnet as the default unless a stage genuinely needs Opus

This is a 30-minute audit that could save you significant budget headroom for the rest of summer.

🔗 https://www.mindstudio.ai/blog/ai-model-routing-fable-5-opus-sonnet-haiku
🔗 https://www.developersdigest.tech/blog/best-claude-model-after-fable-5
