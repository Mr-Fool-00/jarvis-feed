# Briefing: MAGNET + ATLAS — Multi-Agent Character-Grounded Story Architecture

**Date:** 2026-07-04  
**Source:** arxiv:2607.00918  
**Score:** 8/10 · build_worthy: TRUE  
**Relevance:** Direct — multi-agent orchestration + long-form fiction pipeline

---

## The Problem Being Solved

Single-agent story generation degrades across long narratives. The model's context window can't simultaneously track:
- Plot continuity (what happened, where, when, why)
- Character voice consistency (how each character speaks, thinks, acts)
- Narrative architecture (act structure, foreshadowing, theme threads)

Existing fixes treat this as a **retrieval problem**: stuff character sheets and summaries into context. MAGNET/ATLAS treats it as an **isolation problem**: stop asking one agent to hold all state at once.

---

## The Architecture

```
                    ┌─────────────┐
                    │   ATLAS     │  ← supervisor: routes tasks,
                    │  Supervisor │    resolves conflicts, manages
                    └──────┬──────┘    agent lifecycle
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
  ┌──────────┐      ┌────────────┐      ┌──────────────┐
  │ Narrator │      │ Character  │      │  Continuity  │
  │  Agent   │      │ Agents     │      │   Auditor    │
  │          │      │ (one each) │      │              │
  └──────────┘      └────────────┘      └──────────────┘
```

**Narrator agent** — owns plot sequencing, scene structure, pacing. Writes the actual prose once all character contributions are resolved.

**Character agents** — one per major character. Each receives *only* that character's accumulated history and perspective. No cross-contamination. Character A's agent never sees what Character B's agent is holding. This is the core isolation mechanism.

**Continuity Auditor** — runs a verification pass after each scene before the Narrator advances. Checks: timeline consistency, character location, factual continuity, established rules of the world. Can trigger targeted rewrites of a scene before it becomes canonical.

**ATLAS supervisor** — routes incoming task requests, breaks ties when Character agents conflict, manages the token budget across agents, decides when a scene passes Auditor review.

**Execution order per scene:**
1. ATLAS receives scene brief from Narrator
2. ATLAS dispatches scene context to relevant Character agents
3. Character agents return their character's contribution (dialogue, action, internal state)
4. Narrator drafts scene from contributions
5. Auditor reviews draft — flags violations, returns pass/rewrite
6. If rewrite: targeted revision, re-audit (up to N rounds)
7. Scene committed to canonical narrative; all agents' histories updated

---

## Key Results (from paper)

- **41% reduction in plot continuity errors** vs. single-agent baseline across 150-scene benchmark stories
- **50% reduction in character hallucinations** (character saying/doing things inconsistent with their established history)
- Tested on stories of 30,000–85,000 words
- Character isolation accounted for ~60% of the improvement; Auditor verification accounted for ~40%

---

## Adaptation for Your Stack

You have: Claude Code background subagents, slash skills, JARVIS_PERSONA, Fable 5 for fiction.

### Model routing

| Role | Model | Rationale |
|---|---|---|
| ATLAS supervisor | claude-sonnet-5 | Orchestration, routing logic, conflict resolution |
| Narrator | claude-fable-5 | Prose quality is the output — use the best creative model |
| Character agents | claude-fable-5 | Voice consistency requires high creative fidelity |
| Auditor | claude-sonnet-5 | Analytical verification — Sonnet is faster and cheaper here |

### Slash skill structure

```
/story-scene [scene-brief]
  → dispatches to background character agents (one per active character)
  → collects contributions
  → Narrator drafts
  → Auditor reviews
  → returns committed scene
```

### Character agent initialization

Each character agent gets a system prompt containing only:
```
You are [Name]'s dedicated character agent.

[Name]'s established history (cumulative, updated per scene):
<history>

[Name]'s voice and behavioral rules:
<voice_profile>

When given a scene brief, respond only as [Name] would — their 
dialogue, actions, internal state. Do not reference other characters' 
private thoughts or motivations.
```

### Auditor prompt pattern

```
Review this scene for continuity violations:
<scene_draft>

Canonical facts established in prior scenes:
<continuity_db>

Return: PASS or list of specific violations with line references.
Do not rewrite — flag only.
```

### Continuity database

Maintain a running `continuity.json` in the project directory — a structured record of canonical facts (character locations, events, timestamps, established rules) that the Auditor queries per scene. Simpler than vector retrieval; a JSON append works for novels up to ~100 scenes before you need semantic search.

---

## What to Build First

The highest-ROI starting point is the **Auditor alone** — you can add it as a post-scene hook to your existing single-agent fiction pipeline without restructuring anything else. Run your current pipeline as-is, then route each scene through an Auditor pass before advancing. The 40% continuity improvement from the Auditor is the cheapest win.

Once that's working, add character agent isolation scene by scene.

---

## Caveats

- The paper uses GPT-4o as baseline, not Fable 5. Fable 5's character voice is likely stronger, meaning baseline is already better — the percentage improvements may be smaller but the absolute quality ceiling is higher.
- Per-character agents multiply token cost by the number of active characters per scene. For 5 characters, expect 5x the character-contribution tokens. On Max plan: rate limit throughput concern, not cost.
- ATLAS supervisor logic is the hardest part to implement well — conflict resolution when two character agents return contradictory contributions requires judgment. Start with simple "Narrator decides" arbitration.

---

*Filed by Jarvis · 2026-07-04 AM run*
