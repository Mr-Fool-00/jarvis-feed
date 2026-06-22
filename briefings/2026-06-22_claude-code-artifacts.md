# Claude Code Artifacts — 7/10

## What it is

Anthropic shipped a feature called Artifacts for Claude Code on June 18. When you finish a Claude Code session, you can publish the output as a live, interactive web page. Not a screenshot or a download — an actual URL that updates in real time as Claude keeps working. Your teammates open the URL and watch the dashboard, checklist, or report change live as the agent runs. Everything is locked down in a security sandbox (no external scripts, no network calls, fully self-contained).

It's currently only for Claude Team and Enterprise plans. Not on Max yet.

## Why you'd want it (your stack specifically)

You run multi-agent book pipelines where several agents are working simultaneously. Right now, checking progress means digging into files or watching terminal output. Artifacts would let you spin up a live status page — "Chapter 7: planning done, drafting 40%, fixer pending" — that updates as the agents work. For Jarvis itself, a live dashboard page showing which routines ran, what they found, what's in the digest — that's genuinely useful.

You can't use the official feature yet (it's Team/Enterprise), but the pattern is worth building a lightweight analog of right now. A simple HTML file your agents write to as they run is basically the same thing.

## Why I think it's worth your attention

It's the first time Anthropic has built a "show your work live" interface into CC natively. The direction they're going — agents that produce live-updating artifacts rather than just files — is where your own pipeline should go too. You're building a reader-facing book product. A live production dashboard during runs could save you a lot of "wait, what's it doing?" interruptions.

## What to do

A. If you want the official feature: You'd need to move from Max ($200/mo individual) to Claude Team (~$30/seat/mo, minimum 5 seats = $150/mo). Probably not worth it right now just for Artifacts.

B. The better move: Build a dead-simple HTML dashboard your agents write to as a side-effect. One JSON file that agents update per chapter, one HTML template that reads it. You get the same mental model without needing Team plan. Tell me if you want me to sketch this out.

🔗 https://claude.com/blog/artifacts-in-claude-code
🔗 https://venturebeat.com/data/anthropics-claude-code-artifacts-update-brings-live-shared-dashboards-and-interactive-workspaces-to-enterprises
