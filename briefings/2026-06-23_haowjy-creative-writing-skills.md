# Briefing: haowjy/creative-writing-skills v0.4.0 — 14-Agent Creative Writing Suite

**Date:** 2026-06-23  
**Score:** 7/10  
**Type:** B — Third-party code (safety gate applied)  
**Channel:** #improvements

---

## Safety gate result: DEEP-DIVE COMPLETED ✅

- **Stars:** 272 (sufficient for initial review, not enough for auto-install)  
- **License:** Apache 2.0  
- **CI/CD:** Present  
- **External dependencies:** None unusual — no external API calls, no postinstall scripts  
- **Code review:** Clean structure, standard Claude Code skill patterns  
- **Safety verdict:** No red flags. Pattern is safe to study and build from. **DO NOT install directly — build our own version.**

---

## What it is

Released v0.4.0 on June 21, 2026. A 14-agent creative writing suite designed to work across Claude Code, Cowork, and Claude.ai.

**Agents:**
- `muse` — author-facing creative partner
- `bard` — drafting orchestrator
- `writer` — prose generation
- `critic` — internal reviewer
- `reader-sim` — simulates reader reactions on draft scenes
- `character-sim` — character voice testing
- `continuity-checker` — cross-chapter consistency enforcement
- `brainstormer` — ideation partner
- `outliner` — structure planning
- `style-creator` — voice DNA file generation
- `chronicler` — timeline and event tracking
- + additional utility agents

**Skills (18):** prose technique, scene construction, critique methodology, character voice, reader experience, project setup.

---

## Why it matters for your pipeline

Your current book pipeline has a known drift problem: each chapter starts fresh and loses consistency with prior chapters. haowjy's `continuity-checker` + `chronicler` combo directly addresses this.

Two patterns in particular are worth building into your pipeline:

**1. Voice DNA file:** `style-creator` generates a persistent voice reference file at project start. Every subsequent agent reads it before generating. This is the structural fix for voice drift, not a prompt-level fix.

**2. reader-sim agent:** Runs simulated reader reactions on a draft scene before committing. "Would a reader find this confusing? Boring? Emotionally satisfying?" — this is the quality gate pattern missing from your current flow. The character-sim extension (would this character plausibly do this?) is another layer worth adding.

**Pipeline pattern to build:** `muse → bard → writer → continuity-checker` with a `voice-DNA.md` file and per-chapter quality gates calibrated to your style. The reader-sim gate is the novel idea here — it's a cheap insurance pass before a chapter gets committed to the manuscript.

---

## What to build (your version, not an install)

1. **voice-DNA.md** — a style reference file you manually curate for your book (or generate with a one-shot session). Checked in to the book repo. Every writing agent reads it at start.

2. **continuity-checker agent** — reads `voice-DNA.md` + last 3 chapter summaries before reviewing a new chapter for drift. Returns a PASS/FAIL + specific drift flags.

3. **reader-sim agent** — given a draft scene, asks: "What would a reader find confusing here? Where does pacing drag? Is the emotional beat landing?" Returns structured feedback before chapter is committed.

4. **quality gate loop** — chapter draft → continuity-checker → reader-sim → if FAIL, revise → re-run → commit. The DEV.to thresholds from digest item #5 (30-40% dialogue, 15+ sensory details/1000w, <80w avg paragraph) are your numeric targets.

Leo approval needed before building. This is a medium-complexity build — 2-3 sessions.

---

## Sources

- https://github.com/haowjy/creative-writing-skills (v0.4.0, June 21 2026)

---

**Build verdict:** BUILDABLE — Build our own muse→bard→writer→continuity-checker pipeline. Key patterns: voice-DNA file, reader-sim quality gate, per-chapter drift checking. **Awaiting Leo approval.**
