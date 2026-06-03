# NousResearch/autonovel — 8/10

## What it is

Nous Research (an AI research org) published an open-source system that takes a story idea and turns it into a complete novel — automatically. It builds the world and characters, writes each chapter one by one, grades each chapter and rewrites bad ones, runs the full manuscript through two AI "editors" arguing about quality, then exports to PDF, ebook, and audiobook. Their first novel out of the system was 79,456 words long. Not a toy. It actually shipped a book.

## Why you'd want it (specific to your stack)

Your writing pipeline right now generates chapters. What it doesn't have is the adversarial review step — where one AI plays "harsh literary critic" and another plays "fiction professor" and they fight over what's wrong with the manuscript. That fight is what separates a fast AI draft from something that reads like a real book.

Also: the repo has a file called ANTI-SLOP.md with 21 words that instantly signal AI writing (delve, leverage, tapestry, paradigm, nuanced, realm, myriad, plethora — the whole list). And a list of structural tells: "symmetry addiction" (AI always writes 3 bullet points), "hedge parades" (may/might/could everywhere), "em dash overload." You can copy this entire list directly into your fiction CLAUDE.md right now and Claude will avoid them automatically. No code. Five minutes.

For the 10-15 books this summer: the quality gate (retry the chapter if score < 6.0) + the dual-reviewer pattern are the two missing pieces between "AI generated this" and "I'd publish this." Both are buildable.

## Why I think it's worth your attention

This is the clearest working reference I've seen for what your `/book-pipeline` skill should look like when it's actually done. Not a tutorial, not a pattern paper — a running system with a proof novel.

## What to do

Two things, in order:

**Right now (5 min):** Open your fiction CLAUDE.md or whatever file your writing agent reads at startup. Paste in the ANTI-SLOP word list and the structural anti-patterns from this file. Immediate quality improvement, zero pipeline changes.

**This week:** Read the PIPELINE.md in the repo. It's the architecture reference for the `/book-pipeline` skill we've been building toward — the foundation loop, quality-gated chapter retry, and dual-persona review pattern. Then approve or reject building a native version of this for your stack.

🔗 https://github.com/NousResearch/autonovel
🔗 https://github.com/NousResearch/autonovel/blob/master/ANTI-SLOP.md
🔗 https://github.com/NousResearch/autonovel/blob/master/PIPELINE.md
🔗 https://nousresearch.com/bells (the proof novel)
