# Briefing: Narrative Flattening — Why AI Fiction Feels Flat, and What to Do About It

**Date:** 2026-07-03
**Score:** 7/10
**Paper:** arXiv:2605.27878 (published May 27, 2026)
**Authors:** Knowledge Lab, University of Chicago
**Verdict:** BUILD-WORTHY — diagnostic framework for the "why does this feel hollow" problem in AI fiction. Changes model selection logic.

---

## What is narrative flattening?

It's a name for something you've probably noticed: AI-generated fiction can be grammatically perfect, logically consistent, and emotionally inert. Competent sentences, no soul.

This paper proves that's not an accident. It's a measurable, structural consequence of how LLMs are trained after their initial pre-training.

**Narrative flattening** = post-training compresses the dynamic range of a model's fiction output. Less thematic variation. Less emotional intensity. Less stylistic diversity across different stories. The model converges to a house style.

---

## How they proved it

They took OLMo 32B (a publicly available model) and tested four versions of it:
- **Base**: the raw pre-trained model, before any fine-tuning
- **SFT**: supervised fine-tuning (the "helpful assistant" stage)
- **DPO**: direct preference optimization (first RLHF-family stage)
- **RLVR**: reinforcement learning with verifiable rewards (final RL stage)

They had each version continue stories from three different corpora — casual platform fiction, instructed stories, and professional literary fiction (The New Yorker). Then they measured:

1. **Thematic motion**: does the thematic content shift as the story progresses, or does it stay flat?
2. **Affective prevalence**: how much emotional content? How intense?
3. **Linguistic diversity**: do different stories feel stylistically different, or do they all sound the same?

**Result across all three measures:** every post-training stage makes things flatter. Base is most diverse. SFT starts the collapse. DPO and RLVR continue it. The RL stage can claw back about 5% of lost diversity — but only if the model hasn't already fully collapsed going into that stage.

---

## What this means for your fiction pipeline

**The model you're probably using is instruction-tuned.** Claude Sonnet 5, Opus 4.8, every model you'd reach for in a CC session or API call — they've all been through SFT, RLHF, and Constitutional AI. By the logic of this paper, all of them are operating under narrative flattening by default.

This doesn't mean they can't write good fiction. It means they have a structural bias toward the center — toward emotional moderation, stylistic convergence, thematic predictability.

**Three things you can do with this:**

1. **Prompting against the grain.** Explicitly prompting for emotional extremes, stylistic range, thematic contradiction will help — not because the model "unlocks" hidden diversity, but because you're counteracting the bias. Prompts like "this scene should feel genuinely chaotic, not resolved" or "vary your sentence rhythm aggressively" push against the flattening tendency.

2. **Model selection for diversity runs.** If you can access a base or lightly SFT'd model, it will produce more stylistically varied output — at the cost of being harder to control and more likely to drift off-task. A viable workflow: use a base or lightly-trained model for first-draft generation of key scenes (the emotionally charged ones), then use the instruction-tuned model for revision, consistency, and craft refinement.

3. **Treating this as a calibration point.** When Leo evaluates whether an overnight fiction output "feels flat," this paper gives a framework for diagnosing why: thematic motion (did the chapter go anywhere emotionally?), affective prevalence (was anything at high emotional intensity?), stylistic diversity (does scene 1 read differently from scene 5?). These are the three dimensions where post-training causes the most compression.

---

## The honest limitation

This paper is diagnostic, not prescriptive. It tells you what causes the flatness and which model checkpoints are more or less affected. It doesn't give you a plug-and-play fix. The fix is either accepting the tradeoff (instruction-tuned = more controllable but flatter) or deliberately working against it through prompting strategy and model selection.

🔗 https://arxiv.org/abs/2605.27878
