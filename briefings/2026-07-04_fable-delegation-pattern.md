# Briefing: Fable's Judgement — Model-Dispatch Heuristic for Multi-Agent Pipelines

**Date:** 2026-07-04  
**Source:** Simon Willison, simonwillison.net — July 3, 2026  
**Score:** 7/10 · build_worthy: TRUE  
**Relevance:** Direct — multi-agent orchestration, Max plan model routing

---

## The Idea

In a multi-agent pipeline, the default is inheritance: subagents run on whatever model the supervisor runs on, because that's the path of least resistance. Simon's post argues this is leaving significant quality and cost on the table. The better pattern is **explicit dispatch by task type**:

| Task type | Route to | Why |
|---|---|---|
| Creative writing, fiction, roleplay, voice | Fable 5 | Highest creative fidelity; reasoning overhead is wasted here |
| Code, engineering, analysis, debugging | Sonnet 5 | Best code generation; reasoning makes real differences |
| Extraction, classification, summarization at scale | Haiku 4.5 | Fast and cheap; task doesn't need depth |
| Judgment, orchestration, cross-agent synthesis | Sonnet 5 | Analytical tasks, not creative ones |

The insight is that Fable 5 is not a "better Sonnet" — it's a different tool. Using Fable for engineering tasks (and Sonnet for creative tasks) wastes the model's strengths. The dispatch decision should be made by the supervisor, not inherited by the subagent.

---

## The Pattern in Code

In a Claude Code context using background subagents or Agent() calls:

```python
# Pseudo-code for an ATLAS-style supervisor
def dispatch_task(task: Task) -> str:
    model = {
        "character_voice": "claude-fable-5",
        "prose_generation": "claude-fable-5",
        "narrative_planning": "claude-fable-5",
        "continuity_audit": "claude-sonnet-5",
        "code_generation": "claude-sonnet-5",
        "plot_consistency_check": "claude-sonnet-5",
        "metadata_extraction": "claude-haiku-4-5-20251001",
        "word_count_analysis": "claude-haiku-4-5-20251001",
        "scene_classification": "claude-haiku-4-5-20251001",
    }.get(task.type, "claude-sonnet-5")  # default to Sonnet for unknown tasks
    
    return agent(task.prompt, model=model)
```

In a Jarvis workflow script (for the Workflow tool):

```javascript
// In a workflow script
const CHARACTER_TASKS = ['character_voice', 'dialogue', 'prose'];
const ENGINEERING_TASKS = ['code', 'debug', 'refactor'];
const EXTRACTION_TASKS = ['extract', 'classify', 'summarize'];

function routeModel(taskType) {
  if (CHARACTER_TASKS.some(t => taskType.includes(t))) return 'fable';
  if (ENGINEERING_TASKS.some(t => taskType.includes(t))) return 'sonnet';
  if (EXTRACTION_TASKS.some(t => taskType.includes(t))) return 'haiku';
  return undefined; // inherit session model
}

// In pipeline:
const results = await pipeline(
  tasks,
  task => agent(task.prompt, { 
    label: task.type,
    model: routeModel(task.type)
  })
);
```

---

## Application to Your Fiction Pipeline

Your current Jarvis architecture runs everything on Sonnet (the session model). The dispatch opportunity is in three places:

**1. Character voice generation**  
Any subagent writing *as* a character (dialogue, internal monologue, character-specific narration) should run on Fable 5. This is where the model's creative fidelity matters most and where Sonnet's engineering orientation adds no value.

**2. World-building and prose drafting**  
Scene descriptions, thematic passages, narrative voice — Fable 5. The model was specifically trained to excel here.

**3. Auditing, verification, planning**  
Continuity checks, outline validation, plot-hole detection, character arc analysis — these are analytical tasks. Sonnet 5 is faster and more precise for structured verification work.

**Estimated quality uplift:** The biggest gain is in character voice. If you run a 5-character scene where each character's contribution currently comes from a Sonnet subagent, switching those to Fable subagents should meaningfully improve voice distinctiveness and reduce drift toward generic "assistant-speak."

**On Max plan:** No cost difference — both models are included. The only constraint is throughput. Fable subagents may queue differently than Sonnet ones depending on how Anthropic allocates Max plan capacity per model family.

---

## One-File Implementation

The dispatch logic is a single module that belongs in your Jarvis pipeline infrastructure:

```python
# jarvis/model_router.py

FABLE = "claude-fable-5"
SONNET = "claude-sonnet-5"  
HAIKU = "claude-haiku-4-5-20251001"

_FICTION_SIGNALS = {"voice", "dialogue", "prose", "character", "creative", "narrative", "fiction"}
_HAIKU_SIGNALS = {"extract", "classify", "summarize", "count", "parse", "chunk"}

def route(task_description: str, default=SONNET) -> str:
    desc = task_description.lower()
    if any(s in desc for s in _FICTION_SIGNALS):
        return FABLE
    if any(s in desc for s in _HAIKU_SIGNALS):
        return HAIKU
    return default
```

Use `route(task_description)` when constructing subagent calls. The heuristic is intentionally simple — refine the signal sets based on observed output quality over time.

---

## What to Watch For

Simon's original post notes that model dispatch works best when the supervisor has a clear taxonomy. The failure mode is a task that spans categories — "write a plot outline in the protagonist's voice" is both creative and analytical. For hybrid tasks, default to Fable (creative quality is harder to recover than analytical precision) and verify with a Sonnet audit pass if needed.

---

*Filed by Jarvis · 2026-07-04 AM run*
