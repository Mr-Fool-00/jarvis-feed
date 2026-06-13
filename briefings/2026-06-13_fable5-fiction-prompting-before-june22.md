# Fable 5 Fiction Prompting — Before June 22 — 7/10

## What it is
Fable 5 (released June 9) is free on your Max plan until June 22 — 8 days from today. After June 22 it costs $10/M input + $50/M output (roughly 3× Sonnet 4.6). The model is Mythos-class: better long-horizon reasoning, writes longer chapters, and is proactively autonomous in a way Opus 4.8 isn't. But it requires a different prompting approach to get the most out of it before you're paying for it.

## Why you'd want it (specific to your stack)
Three things directly relevant to your fiction pipeline:

1. **Goal-level prompting beats step-by-step.** Fable 5 runs an entire workflow in one turn if you tell it what success looks like. Instead of "now write scene 1, then scene 2..." you say: "Write chapter 3 of the attached novel. Success = 4,000 words, each scene ends with a beat that pulls to the next, Leo's voice DNA file is the style target. Stop and ask me only if plot contradicts story.md." Fable handles the rest.

2. **It writes longer than Opus 4.8.** It trends toward using the full word budget. You probably want to set explicit chapter length targets in CLAUDE.md to avoid 7,000-word chapters when you wanted 4,000.

3. **"Relentlessly proactive" means tighter constraints matter.** Simon Willison showed a screenshot of a bug and Fable 5 spun up its own CORS servers unasked. For fiction, this means Fable will make character decisions, fill plot gaps, and extend scenes without asking. That's great for throughput, but your CLAUDE.md needs explicit "ASK before: [changing character relationships, introducing new plot elements, killing characters]" rules, or Fable will just do it.

**⚠️ Privacy note:** Fable 5 retains all prompts and outputs for 30 days on all platforms — including Max plan. Your novel drafts and world-building notes will be stored. This is different from Sonnet/Opus. Know this before sending your full story bible to Fable 5.

## Why I think it's worth your attention
You have 8 free days to test the best model Anthropic has ever shipped publicly. Even if you decide it's too expensive for the full pipeline post-June-22, one or two well-designed test chapters will tell you if the prose quality jump is worth the cost, and give you calibration for when to use it vs Sonnet.

## What to do
Before June 22, run this specific test:
1. Take a chapter from your current best pipeline output (your Sonnet 4.6 best work)
2. Run the same chapter prompt through Fable 5 with goal-level prompting: describe the chapter goal, character beats, word target, voice file reference
3. Compare: Does Fable 5's prose quality justify 3× cost? Where does it drift without your CLAUDE.md constraints?
4. If YES: build a "Fable 5 fiction mode" toggle in your pipeline for the chapters that matter most (climaxes, key character moments)
5. If NO: you've confirmed Sonnet 4.6 is the right cost-quality tradeoff and can ignore Fable 5 until the price drops

🔗 https://linas.substack.com/p/prompting-claude-fable-5-guide
🔗 https://www.anthropic.com/claude/fable
🔗 https://lushbinary.com/blog/claude-fable-5-prompting-guide/
