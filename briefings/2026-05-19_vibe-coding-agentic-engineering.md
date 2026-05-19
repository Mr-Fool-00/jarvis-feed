# Simon Willison: Vibe Coding → Agentic Engineering — 7/10

## What it is

Simon Willison (one of the most credible voices writing about practical AI tools) published an essay on May 6 about a shift happening in how developers work with Claude Code. The shift: as the tools get more reliable, even careful engineers are no longer reviewing every single line of AI-generated code. Boris Cherny, the person who built Claude Code, says 100% of his production code is now AI-generated.

Simon uses the phrase "agentic engineering" (coined by Andrej Karpathy) to describe this — it's different from "vibe coding" (just prompting and hoping) because you're still directing the work, still reviewing outcomes and behavior, but you're no longer reviewing every intermediate line. The agent handles more; you handle the intent and the verification.

## Why you'd want it (specific to your stack)

You're heading into a 12-week summer where you want to ship 10-15 books, refine Jarvis, and figure out revenue — all simultaneously. That pace only works if you're in agentic-engineering mode, not line-review mode. Reading Simon's framing of why that's defensible (not lazy) is useful context for how to work this summer without second-guessing every Claude output.

The specific tension Simon names — "Claude doesn't have a professional reputation or accountability" — is worth having in your head. The answer isn't to go back to reviewing everything. It's to design your pipelines so that **outcomes** are verified (tests pass, chapter continuity checks pass, world-bible consistency passes) even when line-by-line review isn't.

## Why I think it's worth your attention

This is the operating philosophy paper for the kind of work you're about to do at scale. Worth 10 minutes before finals end.

## What to do

Read the essay. It's short. If it changes how you think about the judge/reviewer/fixer loop structure in your writing pipeline, great. If not, at minimum it validates that working the way you're planning to work this summer is the right call.

🔗 https://simonwillison.net/2026/May/6/vibe-coding-and-agentic-engineering/
