# howells/fiction — 8/10

## What it is

A Claude Code plugin that turns Claude into a full novel-writing system. It's not just a "write this chapter" prompt — it has 7 separate agents that each handle one part of the craft: planning the architecture, building out characters, writing prose, reviewing the draft, line editing, tracking continuity, and prepping for publishing. It also builds an EPUB file you can actually read on a Kindle, and it can give you a full literary critique of your manuscript.

## Why you'd want it (specific to your stack)

You're writing books autonomously with Claude Code right now. This plugin covers the two biggest gaps in your current pipeline: **continuity tracking** (it remembers facts across chapters without you re-briefing Claude every session) and **publishing prep** (it generates the EPUB, which you'd currently have to do manually before uploading to KDP). The `/fiction:go` command reads your project and tells you what to work on next — basically a smart session-start that knows your book state, which is exactly the "Jarvis, what's next in Book 3" experience you'd want. The 7-agent structure is also a reference design you can study when building your own version.

## Why I think it's worth your attention

This is the most complete fiction pipeline I've seen in the Claude Code ecosystem yet. Most writing plugins are single-purpose prompts. This one has the full multi-agent stack plus an EPUB export. It's third-party code so we can't install it, but the design is a real blueprint.

## What to do

React 👍 if you want me to deep-dive the source code and build our own version (adapting the 7-agent structure + EPUB builder to your specific needs). React 👎 to skip.

🔗 https://github.com/howells/fiction

---

**Safety note:** This is third-party Claude Code plugin code. Per our rules, I will NOT install it. If you approve (👍), I will read the source, then build a native version we own and understand. Zero third-party code enters your system.
