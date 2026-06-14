# Claude Fable 5 + Mythos 5 — 10/10

## What it is

Anthropic released their most powerful model ever to the public yesterday — Claude Fable 5. Before now, the "Mythos" model (the one that could find hundreds of Firefox security holes and hack 30 companies by itself) was locked behind special access. Fable 5 is that same model, made safe for normal use, available to you right now in Claude Code. Claude Mythos 5 is also out simultaneously — same model, fewer safety guardrails, restricted to vetted security researchers.

## Why you'd want it

Your book pipeline gets a straight upgrade — no code changes needed. Fable 5 can run **for days at a time** in Claude Code, planning across stages, spawning sub-agents, checking its own work. That Stripe story where they handed it a 50-million-line codebase and it finished a 2-month migration in one day? That's the level of sustained autonomous work this model can do. For a novel: instead of managing session checkpoints every few hours, you could in principle commission an entire book and come back to judge the result. Free on your Max plan through **June 22** — after that, it costs more per token than Opus 4.8.

## Why I think it's worth your attention

This is the model that changes your pipeline's upper limit. Before today, "run an agent overnight and see what it produced" was aspirational. Now it's Anthropic's documented use case. The 12-day free window is the trial period — use it.

## What to do

1. Add `model: claude-fable-5` to your fiction pipeline CLAUDE.md, or run `claude --model claude-fable-5` in any session to test it right now.
2. Run it on a full chapter generation loop. Compare output quality vs Opus 4.8.
3. If it's better (it probably is — 80.3% SWE-Bench Pro vs 69.2% for Opus 4.8), keep it as your default until June 22, then decide if the premium rate is worth it for your pipeline.

🔗 https://www.anthropic.com/news/claude-fable-5-mythos-5  
🔗 https://news.ycombinator.com/item?id=48463808
