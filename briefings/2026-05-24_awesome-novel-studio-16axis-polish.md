# Awesome Novel Studio — 7/10

## What it is
Someone built a 18-agent pipeline specifically for writing web novels from scratch — from the initial concept all the way through polishing and rewriting. The standout feature is a "16-axis quality checker" that reviews every chapter across 16 different dimensions (voice, logic, scene structure, continuity, character consistency, etc.) before it passes to the next stage. It also has a "continuity bridge" agent that reads back everything that happened in previous episodes so the new chapter doesn't contradict anything.

## Why you'd want it (specific to your stack)
Your writing pipeline generates chapters but doesn't have a systematic quality gate. Right now you're relying on Claude to "just know" whether a chapter is good — but there's no formal check. The 16-axis taxonomy is basically a structured critic that runs after every chapter and flags specific failure modes before you accept it. The continuity bridge is the thing your pipeline is probably missing most: an agent that extracts "timeline events, open foreshadowing, character arcs" from all prior chapters and passes that context into each new chapter generation. That's the fix for "Claude forgot what happened three chapters ago."

## Why I think it's worth your attention
The person who built this shipped a real novel with it and got 2,500+ daily views. This isn't theoretical — the pipeline actually ran. The architecture (four distinct phases: design → create → polish → rewrite) maps cleanly onto what your book pipeline needs to become.

## What to do
**React to this briefing in Slack** to approve or pass. If you approve the PATTERN (not the code):
- I'll build a native version of the 16-axis polish agent for your stack — no third-party install
- I'll build a continuity bridge agent that does the lookback-and-extract step
- You'll own the code, it'll match your existing pipeline structure

**If you pass:** I'll drop it and we move on.

DO NOT install this repo directly. It's third-party code and the Korean web novel platform assumptions would need stripping anyway — building our own version is cleaner and safer.

🔗 https://github.com/MJbae/awesome-novel-studio

---
*Safety gate notes (Step 4.5 review):*
- 128 stars, 40 commits — relatively new, low community validation
- Korean web novel platform (Munpia) specific conventions embedded in several agents
- README describes architecture clearly; no obvious red flags (no network calls to unknown domains, no credential handling, no post-install scripts found in the description)
- Agents are markdown-based SKILL.md files — readable, no executable binaries
- Maintainership: single maintainer (MJbae), no org backing — low bus factor
- Initial score 7/10, post-deep-dive score 7/10 (no demoting factors found, but low community adoption reduces confidence)
- Recommendation: APPROVE PATTERN, build our own version
