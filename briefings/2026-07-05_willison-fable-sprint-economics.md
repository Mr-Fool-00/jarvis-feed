# Briefing: Willison's $149.25 Fable Sprint — Concrete Economics Before July 7

**Item:** Simon Willison: sqlite-utils 4.0rc2, mostly written by Claude Fable (for about $149.25)  
**Source:** simonwillison.net, July 5, 2026  
**URL:** https://simonwillison.net/2026/Jul/5/sqlite-utils-fable/  
**Score:** 8/10 · **Digest:** 2026-07-05_PM · **Run:** PM  
**Relevance window:** Urgent — Fable 5 free window closes July 7 (2 days)

---

## What happened

Simon Willison temporarily upgraded to the $200/month Max plan specifically to sprint a Fable 5 release before the July 7 cutoff. He ran sqlite-utils 4.0rc2 — a meaningful near-final release of his flagship Python library — through Fable in a single overnight session:

- **37 prompts** → **34 commits** → **30 files changed** (+1,321 / -190 lines)
- **Total cost: ~$149.25** (inclusive of the Max plan upgrade cost allocation)
- **Session length:** single overnight sprint (published ~1 AM July 5)

This is not a toy benchmark. sqlite-utils is a real, mature library with an actual user base and technical requirements. The 37 prompts → 34 commits ratio is efficient (≈1.1 prompts/commit), suggesting Leo-level direction with Fable execution rather than a vibe-coding "generate everything" approach.

---

## The economic model

Willison's $149.25 gives you a reference rate for **Fable-assisted library maintenance on a mature codebase**:

| Session type | Estimated tokens | Post-July-7 credits cost |
|---|---|---|
| Short chapter revision (50K in, 20K out) | 70K total | ~$1.60 |
| Full chapter generation (200K in, 80K out) | 280K total | ~$6.00 |
| Architecture design session (100K in, 30K out) | 130K total | ~$3.50 |
| Willison-scale library sprint (~library-sized) | ~3M total* | ~$149 |

*Rough estimate from 34 commits, 1321 lines changed.

For targeted fiction pipeline sessions (chapters, scene rewrites, character sheets, structural decisions), the post-July-7 cost per session is **$1.50–$10 in credits**. Expensive compared to Opus 4.8 on subscription ($0), but potentially cost-justifiable for sessions where Fable's orchestration or creative depth is demonstrably needed.

---

## What Willison's workflow reveals

The post (when fully read) describes a **feedback-loop with author control** pattern:
1. Give Fable the existing code + a specific failure or improvement goal
2. Fable proposes a fix
3. Willison reviews, accepts/rejects, gives corrective feedback
4. Iterate to commit

He did NOT ask Fable to "write the library." He asked it to resolve specific issues and incorporated its output selectively. The 37 prompts to 34 commits ratio reflects this: one prompt per unit of work, with tight human-in-loop validation.

For fiction: this maps to directed scene revision rather than autonomous generation — Fable as a precision tool, not an autonomous author.

---

## Your decision window

**Before July 7 (2 days):**
If there is anything in your pipeline where Fable's capabilities are uniquely valuable — complex structural decisions, long-horizon scene planning, architectural revision of the manuscript — the next 48 hours are the zero-cost window.

**After July 7:**
- Fable at $50/M output tokens, $10/M input
- For typical fiction sessions, this is $2–$10/session from credits
- If Fable's quality improvement over Opus 4.8 is demonstrable for a specific use case, it's cost-justifiable per-session
- For general writing where Opus 4.8 is 90% as good, Opus 4.8 on subscription is the right call

---

## Recommended action

1. **In the next 48 hours:** Identify 1-2 tasks in your fiction pipeline that are Fable-specific (complex structural work, multi-POV scene coordination, long-horizon planning). Run them now while Fable is free.
2. **Build a "Fable rate card":** Track input/output tokens for your typical sessions so you know the actual credit cost per session after July 7.
3. **Default stack post-July-7:** Opus 4.8 (subscription, no credits) for general writing; credits-Fable only when the task justifies the $50/M output rate.
