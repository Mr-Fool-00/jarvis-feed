# Leo's Interest Profile (Ranking Criteria)

This file is the ranking prompt for the discovery loop. Items get scored 0–10 against it. Edit freely to retune.

## Context — what Leo actually does

- Builds long-form AI writing pipelines (Kindle novels, fanfiction) — chapter-scale generation, voice consistency, multi-agent fixer pipelines
- Builds worldbuilding tooling (Fate-Anchor, primal-survival, shadow-emperor, etc.)
- Operates on Claude Code + Max plan ($200/mo flat) — **never the Anthropic API**
- Cares about cost-per-chapter math, weekly Max-plan-budget consumption
- Loves multi-agent orchestration (judges, councils, fixers, peer reviewers)
- Has a school side track (APUSH, AP Chem, ACT prep) — low priority for this feed
- Uses macOS, Mac, GitHub, occasionally Unity for game dev

## Strong YES — score 7–10

- New Claude Code **skills, agents, plugins, MCPs, hooks**, especially ones with concrete code/repo
- Anthropic **product updates, model launches, Claude API/SDK features** (even if he won't use the API directly, knowing what shipped matters)
- **Multi-agent orchestration patterns**: agent swarms, councils, judge/critique loops, fixer pipelines, peer-review chains
- **Cost-efficient LLM workflows on Max/managed plans** — anyone documenting how to fit production work into a $200/mo plan
- **Long-form AI writing tooling**: story generation, chapter-scale prompting, voice DNA, style transfer, world-bible-aware writing
- **Reverse-engineering posts** that show HOW someone built a working pipeline (not just "look what AI made")
- **Karpathy / Simon Willison / swyx / dair_ai / Ethan Mollick** tier deep takes
- New **papers with named practical implication** for prompting or agent design (skip pure theory unless implication is concrete)
- Tooling for **memory / RAG / vector DBs** that's lightweight enough to run on a personal machine
- Posts about **prompting tricks specifically for Claude Sonnet/Opus** that show measurable improvement

## Medium — score 4–6

- Generic prompt engineering tips
- Vibe-coding demos (interesting but not directly actionable)
- New AI startups / funding news (only if product has technical novelty)
- ChatGPT-specific tips that **generalize** to Claude
- General programming tooling that pairs well with Claude Code
- AI research papers without clear practical implication

## Strong NO — score 0–2 (filter out)

- "10 AI tools you NEED" listicles
- AI-replaces-job / AI-doomer takes
- Crypto / web3 / NFT integrations
- Sales / cold-email / marketing automation
- "I made $X with AI" hustle content
- No-code wrapper apps with no novel layer
- ChatGPT-specific tips that DON'T generalize
- Recycled news (story already in last 14 days of `seen.json`)
- Tutorials for tools Leo would never use (Bubble.io, Zapier-only flows)

## Hard ignore — drop entirely (don't even surface low-scored)

- NSFW / political / true crime
- Affiliate-link-stuffed articles
- Sponsored / promo posts
- Pure self-promotion with no novel content

## Output style for digest items

Each surfaced item should include:

1. **Title** (link)
2. **Source** + score
3. **One-paragraph summary** (3-4 lines, no fluff)
4. **Why it matters to me** — explicit tie to one of Leo's projects (writing pipeline, Claude Code automation, worldbuilding tooling, multi-agent orchestration). If you can't tie it, lower the score and skip.

Example:
> ### 3. Karpathy releases nanochat
> **Source:** github.com/karpathy/nanochat · **Score:** 9/10
> Minimal Claude-style chat implementation in ~300 lines. Shows the actual decoder loop with sampling tricks.
> **Why it matters:** Direct reference for your Council skill internals — if you ever want to inspect what's happening inside the agent loop, this is the shortest reproducible example. Could also seed a "build your own micro-agent" skill.
> 🔗 https://github.com/karpathy/nanochat
