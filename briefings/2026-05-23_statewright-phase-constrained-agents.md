# Statewright — Phase-Constrained Tool Access for AI Agents — 8/10

## What it is

A tool that forces your AI agent to follow a defined workflow. You set up "phases" — like Plan, then Code, then Test, then Review — and the agent is only allowed to use certain tools in each phase. During Plan it can only read files. During Test it can only run tests. It literally cannot edit code during the review phase. A Rust engine enforces this without any LLM in the loop — it's deterministic.

## Why you'd want it

Right now your Claude Code writing pipeline sometimes goes sideways — the agent re-reads the same file five times, or starts editing mid-review, or gets confused about what phase it's in. Statewright would enforce that your book-writing agent stays in its lane. When it's in "write chapter" mode, it writes. When it's in "quality check" mode, it reads but can't edit. No more chaotic loops. The research shows it took two local models from 2 out of 10 tasks passing to 10 out of 10 just by adding these constraints.

## Why I think it's worth your attention

It's not hype — the improvement was measured on a real coding benchmark. The core engine is open-source (Apache 2.0), so there's no lock-in. And the concept is something we could build into Jarvis natively: a hook that tracks which phase your writing pipeline is in and blocks off-phase tool use, without installing their software at all.

## What to do

This is third-party code, so I don't install it. But it directly inspired a build idea: we could create a `phase-manager` hook for your writing pipeline that tracks the current phase (planning / writing / fixing / reviewing) and rejects tool calls that don't belong in that phase. No external dependency. Entirely yours.

React 🚀 if you want me to build the native Jarvis phase-manager hook. React 👍 to approve the direction. React 👎 if this isn't worth the effort right now.

🔗 https://github.com/statewright/statewright
🔗 Show HN: https://news.ycombinator.com/item?id=48108778
