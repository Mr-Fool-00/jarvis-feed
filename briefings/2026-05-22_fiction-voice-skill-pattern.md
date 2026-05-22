# Fiction Voice Skill Pattern (via Nicolas Cole) — 7/10

## What it is
A method for teaching Claude your writing voice once, in a dedicated skill file, so it applies your voice automatically whenever it writes fiction — without you having to paste style rules into every session. You extract your voice rules (sentence length preferences, how your characters talk, what words you never use, what emotional register you write in), put them in a structured skill file, and Claude Cowork loads it contextually when writing. Nicolas Cole is using this to publish 6 novellas in 6 days with consistent voice across all of them.

## Why you'd want it
Right now your writing pipeline reinjects style guidance through CLAUDE.md and chapter context files. That works, but the voice rules are mixed in with everything else. A dedicated voice skill file separates "how Leo's fiction sounds" from "what this chapter needs to do" — cleaner, and it survives context compression better because it's a separate skill that re-injects rather than a line in a prompt that might get summarized away. For Fate-Anchor especially, where you need consistent voice across a long series, this is the right architecture.

## Why I think it's worth your attention
This is the missing piece in your writing pipeline's skill structure. You have agent logic (fixer chain, verify-15), chapter state (story-bible.json), and instructions (CLAUDE.md). You don't have an explicit voice skill. Cole's approach is practical and immediately buildable.

## What to do
I can build a `voice-dna.skill.md` template for your writing pipeline right now — just say the word. The build is: (1) extract your voice rules from existing chapters you're proud of, (2) structure them as a skill file with frontmatter that triggers on fiction-writing tasks, (3) test against a chapter draft and compare before/after. No third-party install needed — this is a skill file we write from scratch.

React 🚀 to this briefing if you want me to build the voice skill template. React 👍 to note it. React 👎 to skip.

🔗 https://nicolascolefiction.substack.com/p/how-to-build-a-claude-cowork-skill
🔗 AI Publishing House context: https://nicolascolefiction.substack.com/p/i-built-an-ai-publishing-house
