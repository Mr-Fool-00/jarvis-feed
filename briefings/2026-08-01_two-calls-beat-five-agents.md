# Two Calls Beat Five Agents — 8/10

## What it is

A new research paper tested whether having a bunch of specialized AI agents take turns (one drafts, one critiques, one fixes, etc.) is actually better than just having one AI do the work and then review its own output once. The answer for the model they tested: two calls — write, then critique/rewrite — beat the five-agent pipeline on math and coding benchmarks, using 7.4× fewer tokens.

The catch: they tested a small local 7B model, not Claude. Claude is a frontier model and may benefit more from specialization than a 7B model does. But the finding still challenges a real assumption.

## Why you'd want it (specific to your stack)

Your fiction writing pipeline has multiple agents in sequence: chapter drafter → voice checker → continuity fixer → critic → editor. Before you keep adding roles to that chain, this paper says: test whether a simpler two-pass loop (draft → self-critique-and-rewrite) gets you 90% of the quality at a fraction of the complexity. If it does, you've cut your pipeline to two agents instead of five and gained speed and cost savings. If it doesn't, you've confirmed that the extra roles are actually earning their keep on your specific task.

The paper also found that the format agents use to talk to each other matters a lot. When agents passed JSON between themselves, accuracy collapsed 30 points because the model couldn't reliably follow the format constraints. Plain text worked much better. Worth checking what your pipeline passes between agents.

## Why I think it's worth your attention

Multi-agent pipeline design is one of the biggest active guesses in how people use Claude Code. Almost nobody has tested whether their specific role structure is actually necessary, or whether it's complexity theater that sounds right but underperforms simpler loops. This paper gives you a concrete test you can run on your own pipeline in an afternoon.

## What to do

Run an A/B: take one chapter of your current book pipeline, run it through your full multi-role chain, then run it through a 2-call self-refinement loop (draft → self-critique-and-rewrite) and compare. If the 2-call version is within 10-15% of quality, simplify.

🔗 https://arxiv.org/abs/2607.26922
