# Briefing: Novel Writing Structure for ClaudeCode

**Date:** 2026-06-21
**Score:** 7/10
**Build verdict:** BUILDABLE — template skeleton + optional CC sub-agent skill
**Source:** note.com/x2775co (Japanese, June 2026)
**Channel:** #improvements

---

## The Core Insight

AI long-form writing fails structurally, not at the prompt level.

Human novelists carry implicit structures in their heads: memory of every character detail, a lived sense of what has "already happened," a felt discipline about whose POV is active. Claude has none of that — it has only the tokens in context, and they scroll out as chapters accumulate.

The fix isn't better prompting. It's **writing environment design**: freeze the world before writing starts, then keep the frozen files as Claude's persistent memory.

---

## The Template Structure

```
novel-project/
├── world/
│   └── settings.md          # Geography, climate, social rules, factions. LOCKED at start.
├── characters/
│   ├── protagonist.md       # Appearance, voice, backstory, contradictions, speech.
│   └── antagonist.md        # One file per named character. APPEND-ONLY during writing.
├── timeline.md              # Chronological event log. Updated AFTER each chapter.
├── viewpoint.md             # POV + knowledge state per chapter. One line per chapter.
├── plot/
│   └── arcs.md              # Story beats: planned / in-progress / completed.
└── chapters/
    └── ch01.md              # Written content. Never edit world/ during chapter writing.
```

**The critical discipline:**
1. Finish all world/ and characters/ files before writing chapter 1.
2. During chapter writing: NEVER rewrite world/ or characters/ files — append notes only.
3. After each chapter: update timeline.md and viewpoint.md.
4. Refer Claude explicitly to these files at the start of every writing session.

---

## Why This Works

This is the same principle Boris Cherny identified in Loop Engineering (June 19, 7/10): **environment beats prompting**. You're not asking Claude to remember — you're giving Claude a stable external environment to read from.

The pattern also maps to how CC's own sub-agents are designed: Plan, Explore, and Task agents each read a constrained set of files rather than trying to hold everything in context.

---

## Build Paths

### Minimal (1–2 hours)
- Create the folder skeleton as a GitHub template repo or CC project template
- Write a `WRITING_START.md` checklist that forces the author to complete all freeze files before `chapters/` exists
- Done — no code required

### Full (4–6 hours)
- Build a CC sub-agent skill: `freeze-check` that validates all required files exist and have minimum content before allowing chapter writing
- Add a `chapter-close` skill that auto-updates timeline.md and viewpoint.md after a chapter session
- Optional: `drift-check` skill that reads current chapter draft against all freeze files and flags inconsistencies

### Integration with Jarvis
- The same freeze-file pattern applies to long Jarvis digest sessions: freeze the interest profile, seen.json snapshot, and runbook summary as context files before the search loop begins
- Already partially implemented — JARVIS_PERSONA.md and INTEREST_PROFILE.md serve this function

---

## Relationship to Prior Items

| Item | Connection |
|------|-----------|
| Boris Cherny Loop Engineering (June 19, 7/10) | "Environment > prompting" — same principle, different domain |
| AdaptOrch topology routing (June 18, 7/10) | Parallel: route writing tasks to the right sub-agent based on phase (freeze vs. write vs. review) |
| SelSkill selective invocation (June 20, 5/10) | When to invoke the drift-check skill vs. skip it |

---

## Suggested Spike

1. Pick one of Leo's existing in-progress writing projects
2. Retroactively create the freeze files from what already exists
3. Run a chapter session with explicit `Read` calls to world/ + characters/ at the start
4. Check whether drift decreases vs. prior sessions

Time: ~2 hours total including template creation.
