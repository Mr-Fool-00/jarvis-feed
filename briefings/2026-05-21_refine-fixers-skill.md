# /refine-fixers — Build from Neil Kakkar's review-checklist pattern — 7/10

## What it is

Neil Kakkar posted a well-read piece this week on how he uses Claude Code. The most interesting idea buried in the middle: he gets Claude to review his code, then instead of just accepting the feedback, he abstracts it into a reusable checklist. That checklist then primes future Claude agents during code review. Over time, the checklist gets better as more patterns get caught.

He called it "review-checklist abstraction." I'm calling it `/refine-fixers` for your stack.

## Why you'd want it (specific to your stack)

Your verify-15 + fixer chain runs on every chapter. Each fixer catches specific types of problems: omniscience violations, filter-phrase creep, body-language bloat, etc.

Right now, those fixers are static. They're as good as when you wrote them. But they're running on 10, 20, 30 chapters of actual catches — and that data goes nowhere. The catches don't feed back into improving the fixer prompts.

A `/refine-fixers` skill would change that. After a batch of chapters, you run it once. It reads the fixer logs, finds recurring patterns the current prompts are struggling with, and proposes specific improvements to each fixer's instructions. You review and approve each change. The chain gets smarter over the course of your summer.

This is how your writing pipeline gets 10% better with each book rather than staying flat.

## Why I think it's worth your attention

You're doing 10-15 books this summer. If the fixer chain compounds — each book making it slightly better — the quality difference between book 1 and book 15 is massive. Without this, the chain is static. With it, it learns from its own catches.

## What to do

Post-finals build. One skill file that:
1. Reads recent fixer-run logs (or verify-15 output files if you're logging them)
2. Groups patterns by fixer
3. Proposes specific prompt additions in plain language
4. Waits for your approval per suggestion before touching anything

No third-party code. Pure Claude Code skill. Leo approves each prompt change. The fixer chain stays under your control.

🔗 https://neilkakkar.com/productive-with-claude-code.html
