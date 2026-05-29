# Mythos 1 Preview — 7/10

## What it is
Anthropic has a restricted, research-only model called Claude Mythos. It's been used internally to find security vulnerabilities in software — it found 10,000+ critical bugs across 281 open-source projects so far. Until now it's been locked down: no public access, only Anthropic researchers.

That's changing. Alongside the Opus 4.8 release yesterday, Anthropic said "we expect to bring Mythos-class models to all customers in the coming weeks." People already spotted a button labeled `claude-mythos-1-preview` appearing in Claude Code.

## Why you'd want it (specific to your stack)
Two things matter here for you:

1. **It's probably already in Opus 4.8.** Anthropic specifically said Opus 4.8's honesty and deception rates are "similar to Mythos Preview." That means you're already getting some of what Mythos will bring — the gap between the publicly available model and the restricted one is closing.

2. **When it drops, you want to be the first to try it for fiction.** Mythos was trained with stronger reasoning and honesty constraints — specifically designed for tasks where getting things wrong is costly. Long-form fiction, where continuity errors and voice drift compound across chapters, fits that profile exactly. If Mythos is better at holding onto constraints over 100K words, it's your book pipeline model.

## Why I think it's worth your attention
This is a heads-up to stay ready. The gap you'd have to close when Mythos ships publicly: one model name change. Your CLAUDE.md and skills should already be model-agnostic (they are) so the upgrade is trivial. The edge is having the right prompting patterns for the new model before everyone else does.

## What to do
Nothing right now. Watch the Anthropic blog. When Mythos 1 goes live:
1. Update `claude-opus-4-8` → `claude-mythos-1` in your CLAUDE.md
2. Run a chapter-generation test on your current pipeline
3. Compare output quality against 4.8 before committing

🔗 https://cryptobriefing.com/claude-opus-4-8-upgrade/
