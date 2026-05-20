# VERDICT: karpathy-autoresearch — SKIPPED

**Score:** 8/10 pre-deep-dive, 5/10 post-deep-dive
**Decision:** SKIP

## Deep-dive findings
AutoResearch is an ML experiment automation loop, not an information research agent. Architecture: read program.md -> propose hypothesis -> edit train.py -> train 5 min -> evaluate val_bpb -> git commit if improved, git reset if not. Single continuous loop on one GPU.

The patterns (git ratchet, time-budget fairness, program.md as orchestration) are clever for ML experimentation but don't map to Leo's `/research` command, which does information research with adversary claim verification.

## Why not BUILD
- The briefing assumed AutoResearch was a research-agent pattern that could improve `/research`. Post-deep-dive, it's ML experiment automation with zero overlap.
- The git ratchet pattern is neat but Leo's fixer chain already does iterative improvement with verification gates.
- Time-budget fairness is a nice constraint but `/research` already has tiered time budgets (Q1: Quick/Standard/Deep/Overnight).
- Building an ML experiment loop would require GPU access and a completely different use case than Leo's stack serves.

## What's admirable
The "program.md as the entire orchestration layer" philosophy and the single-file scope containment are elegant design choices. Worth remembering if Leo ever does GPU experiment work.
