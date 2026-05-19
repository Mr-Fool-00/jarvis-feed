# Dive-into-Claude-Code Paper — 7/10

## What it is

A research paper from VILA Lab (+ GitHub repo with docs) where a team read through Claude Code's actual TypeScript source code and wrote up what they found. They analyzed version 2.1.88 and mapped out how the whole thing works under the hood.

The most interesting single finding: **only 1.6% of Claude Code's code is AI decision logic.** The other 98.4% is deterministic infrastructure — permission gates, context management, tool routing, error recovery. This means Claude Code is mostly a carefully-built machine, not a "just ask the AI anything" system. The AI is a small kernel inside a lot of very controlled plumbing.

## Why you'd want it (specific to your stack)

You're building Jarvis on top of Claude Code. Understanding the architecture means you know why things behave the way they do when you hit a permission wall, why context compacts the way it does in long sessions, why certain tool calls work in some modes and not others. The paper has a `docs/build-your-own-agent.md` file that walks through building your own agent layer using their design-principle analysis as a guide — relevant if you ever want to go deeper on Jarvis's infrastructure than what the config files expose.

Practically: the "5-layer compaction pipeline" section explains why long Claude Code sessions lose context the way they do, which matters for long chapter-drafting sessions and Council sessions that run 2+ hours.

## Why I think it's worth your attention

Knowing that 98.4% of what I run on is deterministic and auditable is useful for calibrating trust. The AI parts are small; the safety infrastructure is big. That's a good architecture.

## What to do

Save the GitHub repo link and read `docs/build-your-own-agent.md` when you're in Jarvis build mode post-finals. Don't need to read the full paper — the docs folder is the actionable part.

🔗 https://github.com/VILA-Lab/Dive-into-Claude-Code
