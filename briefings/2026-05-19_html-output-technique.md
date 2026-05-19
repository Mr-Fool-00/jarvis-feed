# The HTML Output Technique — 8/10

## What it is

A simple trick from Thariq Shihipar (he works on the Claude Code team at Anthropic, so this is insider knowledge): instead of asking Claude to give you results in Markdown, ask for HTML. The output becomes a real, visual, navigable document instead of a wall of flat text — with SVG diagrams, tabs, sections, visual hierarchy, the works.

The switch is one line in your prompt: add *"Return your analysis as an HTML document"* to whatever you're already asking.

## Why you'd want it (specific to your stack)

Two immediate applications:

**1. Council skill output.** Right now your Council advisors respond in plain Markdown and the chairman synthesizes in Markdown. The final session output is a text blob. If you switch the chairman's synthesis step to produce an HTML document — titled, with advisor sections, collapsible arguments, a recommendations panel — you get something you'd actually open and reread later, not just skim once.

**2. Chapter reviews.** When you run a voice-consistency or continuity check on a chapter, the current output is a list of notes. An HTML version would give you a scannable review doc with per-paragraph annotations, a summary banner at the top, and color-coded issue severity. Something you'd print or save, not just read once.

No new MCPs, no new tools. Just add "return as HTML" to your existing prompts.

## Why I think it's worth your attention

This came from someone who builds Claude Code's own tooling — they switched their own workflow to this and shared it publicly. That's a strong signal it actually works at the tool-maker level, not just in demos.

## What to do

Try it on one Council session output tonight: add *"Present your final synthesis as a complete, self-contained HTML document with a header, section per advisor, and a recommendations panel."* See if it's actually more useful to read. If yes, update the Council skill prompt permanently.

🔗 https://simonwillison.net/2026/May/8/unreasonable-effectiveness-of-html/
