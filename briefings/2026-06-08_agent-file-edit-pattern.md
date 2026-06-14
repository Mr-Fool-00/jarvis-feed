# Agent file editing: str_replace/view/insert pattern — 7/10

**Date:** 2026-06-08
**Source URL:** https://simonwillison.net/2026/Jun/7/datasette-agent-edit/
**Score:** 7/10
**Category:** Design pattern (not third-party code — this is a pattern we build natively)

---

## What it is

Simon Willison released a small plugin called datasette-agent-edit on June 7. The whole plugin is 3 tools:

- **view** — show a section of a file with line numbers (e.g., lines 50–80)
- **str_replace** — find an exact old string and replace it with a new string (fails if the string appears more than once, so edits are always precise)
- **insert** — insert text after a specific line number

That's the whole thing. Willison's point: these 3 operations cover essentially every file-editing scenario an agent needs — without giving it the foot-gun of "write the entire file from scratch."

---

## Why you'd want it

Your writing agents currently edit chapters by rewriting the entire file. For a 10,000-word chapter, that means:
- Entire chapter goes into context every time (expensive)
- Any error in the model's output overwrites everything (fragile)
- No way to make a tiny targeted fix without reprocessing the whole chapter

With str_replace/view/insert:
- Agent views the relevant paragraph (10 lines, not 10,000 words)
- Agent replaces only that exact passage
- Everything else in the chapter stays exactly as written
- If the edit fails (string not found), nothing is corrupted — you get an error, not a destroyed file

This is how Claude Code's own Edit tool works internally. It's a validated pattern. Willison just named it and made it a plugin for his own system.

---

## Why I think it's worth your attention

Your current write-the-whole-file approach gets more dangerous as chapters get longer. At 6,000-10,000 words per chapter (which is where your fantasy novels live), a full rewrite on a 2-sentence fix is wasteful and risky. The str_replace pattern makes edits surgical: precise, auditable, and reversible via git diff. It also dramatically reduces token usage for review-and-revise cycles.

---

## What to do

I can build a **chapter-edit skill** for Claude Code with exactly these 3 tools — `chapter:view`, `chapter:str_replace`, `chapter:insert` — as native Bash/JS wrappers. No Datasette dependency. No third-party code. Just the pattern, built for your fiction pipeline.

This would replace the "rewrite chapter from scratch" approach in your current `/revise` flow with targeted surgical edits.

React 👍 to approve and I'll build it as the next skill in your pipeline. It's a half-day build.

🔗 https://simonwillison.net/2026/Jun/7/datasette-agent-edit/
