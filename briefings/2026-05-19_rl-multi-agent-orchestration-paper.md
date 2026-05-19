# RL for Multi-Agent Orchestration (arxiv 2605.02801) — 7/10

## What it is

A research paper (May 2026) that studies the science of multi-agent orchestration — specifically, how to train an AI orchestrator to make good decisions about when to spawn subagents, which subagents to use, how to pass information between them, how to combine their results, and when to decide the work is done.

The paper breaks orchestration into 5 core decisions and 8 reward signals. It uses reinforcement learning to train the orchestrator, but the interesting part for practical builders isn't the RL — it's the **taxonomy** of what orchestration actually consists of.

## Why you'd want it (specific to your stack)

Your Council skill is a multi-agent orchestrator. Right now the orchestration logic lives in the prompt: "ask 5 advisors, then have a chairman synthesize." That works, but it's not principled about *which* advisors to invoke for which problems, how to weight conflicting advice, or when to stop iterating vs. ship the synthesis.

The 5-decision framework from this paper maps directly:
1. **When to spawn** → Council should spawn advisors only when the question has multiple meaningful angles, not for simple queries
2. **Whom to delegate** → advisor selection should match query type (strategy questions get the strategist, writing questions get the editor, etc.)
3. **How to communicate** → how the chairman frames each advisor's brief affects output quality
4. **How to aggregate** → synthesis is a skill, not just concatenation
5. **When to stop** → knowing when another round of advisor input would help vs. is just more noise

Reading this paper gives you the vocabulary to improve the Council skill methodically, not just by feel.

## Why I think it's worth your attention

The research formalizes something you've been doing intuitively. That's useful: it means you can now diagnose Council failures by asking "which of the 5 decisions went wrong?" rather than "why does this feel off?"

## What to do

Skim the abstract and the section on the 5 sub-decisions. If it resonates with patterns you've seen in Council sessions, save it as a reference for the next major Council skill revision post-finals.

🔗 https://arxiv.org/abs/2605.02801
