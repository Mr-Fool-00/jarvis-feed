# Jarvis Self-Suggestions — 2026-06-05 PM

**Run:** 2026-06-05 PM · **Suggestions:** #58–60 · **Routed to:** #improvements

---

## Suggestion #58 — Add IEEE Spectrum AI coverage to tracked sources
**Priority:** Medium | **Effort:** Low

IEEE Spectrum published independent technical analysis of the Anthropic productivity report this run (item #5, score 6/10). The coverage was higher quality than most press — it distinguished LOC-authored vs. LOC-merged, cited Anthropic's own methodology caveats, and mapped autonomy window growth to model releases. IEEE Spectrum consistently produces signal-dense AI/ML coverage that fits the INTEREST_PROFILE.md tech journalism tier.

**Proposed change:** Add `spectrum.ieee.org` to SOURCES.yaml under a new `tech-journalism` WebSearch category, queried for: `site:spectrum.ieee.org (Claude OR "Claude Code" OR "AI agent" OR "large language model")`. Weekly cadence sufficient.

---

## Suggestion #59 — Add Axios AI beat to tracked sources
**Priority:** Low | **Effort:** Low

Axios broke the Anthropic "When AI builds itself" story simultaneously with the institute report (item #10, score 5/10). Axios AI journalists (Ina Fried, Ryan Heath) consistently get embargo access to Anthropic/OpenAI/Google announcements and publish tight, accurate summaries. When WebFetch is blocked on primary sources, Axios often serves as a reliable early-signal fallback.

**Proposed change:** Add Axios AI beat to SOURCES.yaml WebSearch queries: `site:axios.com (Anthropic OR "Claude" OR "AI agent" OR "artificial intelligence") AND (2026)`. Daily cadence, low-noise filter (exclude opinion/politics sections).

---

## Suggestion #60 — Update Step 4.5 to require compositional skill pair auditing
**Priority:** High | **Effort:** Medium

arxiv:2606.00448 ("When Safe Skills Collide: Measuring Compositional Risk in Agent Skill Ecosystems") — surfaced this run at score 7/10 — proves that individual skill safety audits are insufficient. Out of 651 skills that each passed individual inspection, 22.25% of formed pairs have structural compositional risk. The current Step 4.5 safety gate only checks each skill individually.

**Proposed change:** Update AGENT_RUNBOOK.md Step 4.5 to add a compositional audit layer:
> Before recommending any multi-skill configuration (2+ skills loaded simultaneously), enumerate the skill pairs and flag any structural interaction risks. Until a formal pair-audit harness exists, require explicit Leo sign-off on any recommendation involving 3+ third-party skills operating concurrently.

This is directly relevant to any `/book-pipeline` build that loads writer + editor + formatter + safety-checker skills simultaneously.

---

*All three suggestions logged to `state/agent_suggestions.md` as entries #58–60.*
