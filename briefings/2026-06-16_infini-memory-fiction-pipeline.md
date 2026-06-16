# Infini Memory — 7/10

## What it is

A research paper (June 9, 2026) that figured out a better way for AI agents to remember things across long sessions. Instead of keeping one giant log of everything that happened, it organizes memory into separate topic folders — like having a folder called "Characters" with a file for each character, another called "Plot Threads," another for "World Rules." The agent reads only the folders it needs instead of rummaging through everything.

Result: 64.7% accuracy on a standard memory benchmark — meaningfully better than the old "search the whole log" approach.

## Why you'd want it (specific to your stack)

Your book-writing pipeline right now has a memory problem: by chapter 30, the writing agent has no reliable way to remember what happened to a character in chapter 5. It either re-reads everything (slow, expensive) or guesses (consistency errors). Topic-document memory fixes this exactly. Your memory would look like:

```
memory/
  characters/
    Elena.md     ← everything about Elena, updated after each chapter she appears in
    Marcus.md
  plot/
    act-1-resolved.md
    act-2-open-threads.md
  world/
    magic-system.md
    geography.md
```

Each chapter write step: agent updates the relevant files. Each chapter start step: agent reads only the relevant topic files. No searching, no re-reading old chapters. This pairs directly with IS-CoT (already briefed June 15) — IS-CoT handles the Plan-Write-Reflect cycle inside each chapter, Infini Memory handles what gets passed *between* chapters.

## Why I think it's worth your attention

The implementation is just file organization + a "memory update" step added to your chapter-write skill. No new dependencies, no vector databases, no embedding APIs. You could build this in a single afternoon session and it directly addresses the #1 long-form fiction failure mode. The paper benchmarks prove the approach works; the implementation is something you already know how to build.

## What to do

React 🚀 to this briefing to signal "build it." I'll draft the updated chapter-write skill with memory/ integration and the topic-doc consolidation step in the next interactive session. The build sketch:

1. Add `memory/` directory to your novel repo with topic stubs
2. Add a `memory-update` step at the end of `/chapter-write`: agent writes observations to relevant topic files
3. Add a `memory-load` step at the start of `/chapter-write`: agent reads only relevant topic files (not the full log)
4. The IS-CoT Reflect phase feeds the memory-update step naturally

One session to build. Will meaningfully improve chapter-to-chapter consistency on runs longer than 10 chapters.

🔗 https://arxiv.org/abs/2606.10677
