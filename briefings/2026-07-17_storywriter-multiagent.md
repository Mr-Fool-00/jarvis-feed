# StoryWriter: Multi-Agent Long Story Framework — 8/10

## What it is
A four-specialist-agent pipeline for generating long-form stories: Outliner (structure), Drafter (prose), Continuity Checker (contradiction detection), and Revisor (polish). The key design decision: agents communicate through a shared "story bible" document — a structured YAML that holds all plot commitments, character states, and world facts — rather than passing messages to each other directly. This prevents hallucinated continuity.

## Why you'd want it (specific to your stack)
This is the architecture Leo's fiction pipeline needs. The shared story bible is a solved version of the "Claude in chapter 12 contradicts what it wrote in chapter 3" problem. The Continuity Checker agent runs automatically on each new scene and flags contradictions before they compound. You'd implement this as a CC workflow skill with the bible living in `state/story-bible.json`.

## Why I think it's worth your attention
The paper benchmarks against BookSum-style coherence metrics and wins. The "shared state document instead of message passing" pattern also transfers cleanly to any long-running multi-agent pipeline — not just fiction.

## What to do
Read the paper for the architecture, then prototype the story bible schema for your current novel project. Start with the Outliner + Drafter agents; add Continuity Checker once the scaffold is stable.

🔗 https://arxiv.org/abs/2506.16445
