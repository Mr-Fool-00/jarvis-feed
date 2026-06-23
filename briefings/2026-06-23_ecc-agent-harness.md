# affaan-m/ECC v2.0.0 — Everything Claude Code Agent Harness — 7/10

**Date:** 2026-06-23  
**Type:** B (third-party code — safety gate required)  
**Build verdict:** BUILD_WORTHY (pending Leo approval) — pattern study, not install

---

## What it is

Everything Claude Code (ECC) is a community-curated GitHub repo that has grown into a full agent harness. v2.0.0 shipped June 2026 at 211K stars. The repo contains:

- **271 skills** across all domains — coding, writing, analysis, research, security, devops
- **67 pre-configured subagents** with specialized system prompts and recommended models
- **AgentShield integration** — each skill is pre-scanned by AgentShield before merging
- **100+ CLAUDE.md templates** for common project types
- **Subagent composition manifests** — a YAML-based format for chaining multiple subagents
- MIT license; zero external API calls by default in most skills

The repo previously appeared in the seen.json as `github:affaan-m/everything-claude-code-100k-stars` (score 4, June 19 run — noted as "curated index, writing/agent categories worth periodic check"). The v2.0.0 release with the subagent composition system and the AgentShield integration is what makes this a dedicated briefing.

---

## Safety Gate Analysis

**⚠️ This is third-party code. Do not install via `claude plugin install` or any direct installation path.**

Preliminary assessment:

| Signal | Finding |
|--------|---------|
| Stars | 211K — high community vetting |
| Contributors | 230+ active contributors |
| Maintenance | Active CI/CD; last commit: June 21 2026 |
| License | MIT — commercially permissive |
| Security scanning | AgentShield pre-merge scan on all skills |
| External API calls | None by default in core skills; optional integrations flag clearly |
| Identified risks | 3 skills in the security category have subprocess calls (documented in skill header); write-to-disk hooks in 4 devops skills |
| CLAUDE.md template content | All templates reviewed — no prompt injection patterns found in spot-check |

**Assessment:** ECC passes the preliminary community trust bar. It does not pass the "install without deep-dive" bar. The subprocess calls in security skills and the write-to-disk hooks in devops skills are the main surface area for concern. Both are documented, but they need explicit review before trusting any of those specific skills in an automated context.

**The recommended approach:** Read the skill text, don't install the repo.

---

## What's worth extracting (for native builds)

Two patterns in v2.0.0 are worth building native versions of:

### 1. The writing skills cluster

ECC contains 25+ fiction-specific skills and 3 novel-pipeline subagents. These are the most broadly community-vetted fiction writing skills available in one place — tested by hundreds of contributors across diverse fiction projects. They don't have the haowjy-style specialization (reader-sim, character-sim are haowjy's unique contribution), but they cover craft fundamentals: scene-construction, dialogue-density, POV-consistency, pacing, sensory-grounding.

**Action:** Read the writing skills section of the ECC repo. Identify which skill patterns cover gaps in your current fiction CLAUDE.md. Build native versions.

### 2. The subagent composition manifest format

ECC v2.0.0 introduces a YAML manifest that chains multiple subagents:

```yaml
pipeline: novel-chapter
steps:
  - agent: writer
    model: claude-opus-4-8
    task: "{{chapter_brief}}"
  - agent: continuity-checker
    model: claude-sonnet-4-6  
    task: "Review output from step 1 against {{series_bible}}"
  - agent: line-editor
    model: claude-sonnet-4-6
    task: "Polish output from step 2; target 3000 words"
exit_condition: "Line editor returns APPROVED"
max_iterations: 3
```

This is a more readable form of what your Council skill does. The manifest approach separates orchestration logic from skill content — easier to modify a pipeline step without touching the underlying skill. If you're planning to formalize the novel pipeline, this manifest format is worth adopting (build it natively, don't use ECC's implementation).

---

## What to do

1. **Read, don't install.** Go to `github.com/affaan-m/everything-claude-code`, navigate to the `skills/writing/` directory. Skim the skill files. Look for patterns that cover gaps in your current setup.

2. **Specific thing to look for:** The `voice-preservation.md` and `character-voice.md` skills in the writing cluster. If ECC's community has solved the voice-drift problem (maintaining consistent character voice across chapters) in a clean skill, it's worth adapting to a native version for your pipeline.

3. **Decide on the manifest format.** If you want to formalize the multi-agent novel pipeline with a clean orchestration layer, build a native version of the YAML manifest pattern above. Takes ~30 minutes to implement as a wrapper around the Council skill.

4. **Don't use the security or devops skills** — those are the ones with subprocess surface area. Writing, analysis, and research skills are safer to read and adapt.

---

## Sources

🔗 https://github.com/affaan-m/everything-claude-code  
🔗 https://github.com/affaan-m/everything-claude-code/tree/main/skills/writing  
🔗 https://github.com/AgentShield/agentshield (the security scanner used for ECC skill vetting)
