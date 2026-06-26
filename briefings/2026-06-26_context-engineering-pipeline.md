# Context Engineering for AI Agents — 8/10

## What it is

Anthropic's own engineering team published a guide on how to properly load information into an AI agent — and it turns out most people do it wrong. Instead of dumping everything the agent might need at the start (all your character files, world notes, style guides, prior chapters), you load only what each specific step actually needs, right when it needs it. They call this "just-in-time context loading." They also cover how to shrink down old conversation before it crowds out current work.

## Why you'd want it (specific to your stack)

Your chapter writer skill almost certainly loads the entire world bible, all character sheets, and multiple prior chapters at the very start of a session. That's exactly what this guide says to stop doing. The fix is surgical: at the outline step, load only the story arc; at the draft step, load the outline + the two nearest prior chapters + the voice guide; at the revision step, load just the problem passage + the critique. The agent sees a smaller, purpose-built context at each step instead of one massive dump. Anthropic's measured result: 30% task success with upfront loading → 90% with just-in-time loading. That's your chapter quality variance problem in a stat.

## Why I think it's worth your attention

This is the architectural change that makes your writing pipeline actually reliable at scale — 30 chapters in a row without the model losing the thread. The Dreaming + Outcomes quality gate from this morning's run is the WHAT to aim for; this is the HOW to get the agent there reliably. And it's from Anthropic's own engineers, so it's not speculation — they built it into Managed Agents and measured the gain.

## What to do

Read the piece (link below). Then look at your chapter writer skill and list every piece of context it loads at session start. Apply the five-question test from the arxiv companion paper (digest item #3): Is this relevant to THIS step? Does this step actually need it? Does it pollute other steps? Is this the minimal version? Does the agent know where it came from? Anything that fails relevance or economy gets moved to just-in-time loading for the specific step that needs it.

🔗 https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
