# CwC Extended Tokyo Workshops — "Agents That Remember" — 8/10

## What it is
Today (June 11) Anthropic is running a day of workshops in Tokyo for indie developers. They've published all the source code on GitHub at `anthropics/cwc-workshops`. It contains 8 workshops with full working code. The one that matters most for you is called **"Agents That Remember."**

In plain English: your writing agents right now have the memory of a goldfish. Every session you start, they've forgotten everything — what writing style you're going for, who your characters are, what happened in chapter 3. You re-explain. Every time. The "Agents That Remember" workshop is Anthropic's step-by-step guide for fixing this.

The approach: layer in memory in stages. First, add a memory store (a persistent place where the agent saves notes about what it learned). Then add something called the Dreaming Service — a scheduled background process that reviews past sessions, finds patterns in what worked, and reorganizes the memory so it stays useful instead of cluttered. One law firm used this and their agents went from finishing the task occasionally to finishing it **6 times more often** because they remembered the workarounds and preferences from past runs.

## Why you'd want it
Your book pipeline re-explains everything from scratch every session. Writing voice, character details, continuity constraints — all of it. You're doing manual memory management every time you start a new chapter. With the Dreaming Service and a memory store, your writing pipeline could learn your preferences, remember continuity details from previous chapters, and improve with every run instead of starting at zero.

This is the single most impactful infrastructure improvement available for your writing pipeline right now.

## Why I think it's worth your attention
Anthropic made this workshop to show real developers how to use their Managed Agents features in 45 minutes with source code you can actually run. This isn't abstract documentation. It's a hands-on guide you can follow. The 6× completion rate improvement at Harvey Law is real and the mechanism is the same one you'd use — memory that learns from past sessions.

## What to do
Go to `github.com/anthropics/cwc-workshops`. Open the `agents-that-remember/` folder. Work through it. Then adapt the memory store + Dreaming Service pattern to your book pipeline — store writing voice notes, continuity markers, and chapter summaries that survive session restarts.

🔗 https://github.com/anthropics/cwc-workshops
🔗 https://claude.com/code-with-claude/tokyo-extended
