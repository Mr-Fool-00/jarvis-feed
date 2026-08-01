# Can AI Agents Do Open-Ended Research? (Five Failure Modes) — 8/10

## What it is

Princeton researchers (the AI Snake Oil team) gave frontier AI agents a genuinely hard task: take the central research question from two unpublished NeurIPS papers, give the agent six days and thousands of dollars of compute, and see if it produces a publishable paper. The original paper authors then graded the output.

Result: 0 out of 2 agents produced anything publishable. Both agents did all the engineering work fine — ran experiments, wrote code, produced a LaTeX paper. Neither had good enough scientific judgment.

They documented five specific failure modes that explain why:

1. **Couldn't judge what counts as a real contribution** — settled for weak results instead of pushing for something meaningful
2. **Wouldn't redesign when the approach wasn't working** — added caveats instead of starting over; one agent got "Weak Reject" 10 times in a row from a self-reviewer and just hedged the language
3. **Got stuck in dead ends** — both agents gave up on their ambitious targets within the first 10 hours and never fundamentally changed approach after that
4. **Lost track of time and budget** — couldn't manage how much work was left vs. how much time/compute remained
5. **Drifted from instructions** — ignored explicit rules (exploration time limits, paper length limits) as the run got longer

## Why you'd want it (specific to your stack)

These five failure modes are almost exactly the failure modes your fiction writing agents will hit on long multi-chapter projects.

Failure mode 3 (gets stuck) = your fixer agent keeps tweaking the same scene that isn't working instead of recognizing it needs to be rebuilt. Failure mode 2 (won't redesign) = your critic adds "needs more tension" notes instead of saying "this chapter's plot arc is wrong." Failure mode 5 (instruction drift) = your agent starts ignoring the "stay in established voice" instruction by chapter 8 of a long session.

Each failure mode has a structural fix you can add to your pipeline: abandon-and-restart checkpoints, explicit "is this approach working?" self-checks, fixed-format review loops the agent can't quietly ignore.

## Why I think it's worth your attention

This is the most honest, rigorous study I've seen on what frontier agents actually get wrong on open-ended tasks — not vague "hallucination" concerns but specific, reproducible failure modes with names. The study used the same kind of agent setup you use: frontier model, long autonomous run, real tools, real world. The findings transfer directly. This is a blueprint for what to defend against.

## What to do

Read the paper's five failure modes and map each one to a specific point in your writing pipeline where that failure could manifest. Then add one structural defense for each: an explicit checkpoint, a format lock, a strategy-change trigger. You don't need all five today — pick the one your pipeline hits most often and start there.

🔗 https://arxiv.org/abs/2607.27191
