# Advisor Tool — 9/10

## What it is
A new Anthropic API feature that lets a cheaper, faster model (like Sonnet or Haiku) pause mid-task and quietly ask Opus for a smarter plan — all inside a single request. Opus reads everything so far, writes a short course correction (400–700 words), and the executor keeps going. No second call, no extra setup.

## Why you'd want it (specific to your stack)
Your Council skill right now runs separate Opus calls for each advisor — expensive and slow. This collapses that into one. More concretely: your book pipeline could run Haiku for bulk chapter generation (cheap, fast) and let Opus jump in at the craft-critical moments (voice drift, plot logic, pacing). Benchmarks: Haiku + Opus advisor scored **double** Haiku alone on a hard research task. Sonnet + Opus advisor beat Sonnet solo quality at **11.9% lower cost** than running pure Opus. That's a real budget win on your $200/mo plan.

## Why I think it's worth your attention
This is Anthropic shipping the thing you already do manually with your Council pattern — except now it works inside one API request with no orchestration tax. It's the cleanest version of "smart planner, cheap executor" that exists right now.

## What to do
Add to any Claude API call: `betas=["advisor-tool-2026-03-01"]` and put `{"type": "advisor_20260301", "name": "advisor", "model": "claude-opus-4-7"}` in your tools array. The model decides when to ask Opus. Try it in your writing pipeline skill first — it's the highest-impact test.

🔗 https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool
🔗 https://claude.com/blog/the-advisor-strategy
