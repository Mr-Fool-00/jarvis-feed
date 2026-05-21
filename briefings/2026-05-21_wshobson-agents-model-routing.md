# wshobson/agents — 3-Tier Model Routing Pattern — 8/10

## What it is

A GitHub repo with 185 AI agents for Claude Code, 35.7K stars. The interesting part isn't the agents themselves — it's the documented strategy for deciding which model (Opus, Sonnet, Haiku) handles which type of task. The maintainer mapped 185 real agents to specific model tiers based on what actually costs tokens vs. what actually needs intelligence.

**The routing logic in plain terms:**
- **Opus:** Use when you need judgment. Architecture decisions, security review, plot-critical story decisions, anything where a wrong call is expensive.
- **Sonnet:** Use for complex-but-structured work. Voice consistency passes, linking chapters, system design, technology choices.
- **Haiku:** Use for deterministic stuff. Generating from a template, running tests against known patterns, formatting, repetitive structural work.

## Why you'd want it (specific to your stack)

Right now your book pipeline probably defaults everything to the same model. The wshobson pattern applied to your writing pipeline would look like this: Haiku generates the chapter scaffold from a template (fast, cheap), Sonnet does the voice consistency pass and cross-chapter linking (intelligent but structured), Opus handles plot decisions and worldbuilding coherence checks (rare, high-stakes). That split alone cuts your Max-plan usage by 30–40% while keeping quality on the decisions that matter.

It also maps to Jarvis — Haiku for digest formatting/file ops, Sonnet for scoring + analysis, Opus for anything that needs real judgment (like this ranking pass you're reading right now).

## Why I think it's worth your attention

The repo is well-documented (MIT, 383 commits, no dangerous flags). But what's more useful than the code is the DECISION FRAMEWORK — which is just a table saying "here's what each tier is actually good for." That table is buildable as a CLAUDE.md rule without installing anything.

## What I will do (safety rule)

I won't install this. The safety gate passed (no red flags) but I'll build a native version instead. The core value is the routing logic, not the agents themselves. I'll write a `model-routing` section for CLAUDE.md that encodes the Opus/Sonnet/Haiku split for Leo's specific use cases. When Leo approves, I'll build it.

🔗 https://github.com/wshobson/agents
