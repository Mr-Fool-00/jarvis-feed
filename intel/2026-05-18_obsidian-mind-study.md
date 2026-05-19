# obsidian-mind Study Notes — 2026-05-18

## What it is

**Obsidian Mind** by `breferrari` — an Obsidian vault that gives AI coding agents persistent memory across sessions. Multi-LLM: works with Claude Code, Codex CLI, and Gemini CLI from the same vault via shared hooks and slash commands.

Repo: `github.com/breferrari/obsidian-mind` (cloned locally to `~/Desktop/obsidian-mind` for study)

## What it does

The core pattern: an Obsidian vault is the persistent memory layer. Hooks + slash commands automate the read/write of structured memory artifacts.

### Demo flows (from their README)

**Morning kickoff:**
```
/om-standup
→ loads North Star, active projects, open tasks, recent git changes
→ "You have 2 active projects. The auth refactor is blocked on API contract.
   Your 1:1 with Sarah is at 2pm — last time she flagged observability."
```

**Brain dump after a meeting:**
```
/om-dump <freeform paste of meeting notes>
→ Updates org/people/Sarah Chen.md with meeting context
→ Creates work/1-1/Sarah 2026-03-26.md with key takeaways
→ Creates Decision Record: "Defer Redis migration to Q2"
→ Adds to perf/Brag Doc.md: "Auth architecture praised by manager"
→ Updates work/active/Auth Refactor.md with error monitoring task
```

**Incident response:**
```
/om-incident-capture <slack URL>
```

The vault structure (from the cloned directory): `brain/`, `bases/`, plus standard Obsidian dirs (`.obsidian/`), plus custom tooling (`.shardmind/`), plus per-LLM dirs (`.claude/`, `.codex/`, `.gemini/`).

## Honest assessment for Leo's stack

### What's good about it

- ✅ **Truly cross-session memory** — every conversation builds on the last, not "fresh session = zero context"
- ✅ **Multi-LLM portability** — same vault works with Claude Code, Codex, Gemini. Future-proof.
- ✅ **Structured automated capture** — paste a meeting/conversation, it parses into the right files. This is high-value if you do a lot of unstructured-talk → structured-artifact work.
- ✅ **Mature pattern** — the .shardmind tooling, hooks, and command set are well-defined.

### What's NOT a fit for Leo's current state

- ❌ **Requires Obsidian.** Leo doesn't currently use Obsidian. Installing + configuring + learning Obsidian is a real-cost decision, not a quick add.
- ❌ **Memory model conflict with his existing.** Leo's persistent memory is already in: `~/.claude/projects/-Users-leograu/memory/user_profile.md` (behavioral memory), per-project `CLAUDE.md` files (project memory), `jarvis-feed/` repo (Jarvis intelligence), session save files. Adding an Obsidian vault as a 4th layer creates fragmentation, not consolidation.
- ❌ **The work flow it's designed for is engineering-team-life** (1:1s with manager, incident response, sprint work). Leo's actual workflow is more solo-creator (writing books, building Jarvis). The /om-standup output structure is great for "what did the team decide last week" but mismatched for "where am I in chapter 14."

## Cherry-pick: the ideas to steal

Like ruflo, the value is in the ideas, not the full install.

### Idea 1 — `/morning-brief` already does the standup pattern

Tonight I installed `/morning-brief` which is functionally the Claude Code version of `/om-standup` — reads Jarvis digest, calendar, project status, recommends focus block. Same pattern, integrated with Leo's existing storage, no Obsidian needed.

### Idea 2 — A `/dump` equivalent IS worth building (post-finals)

The /om-dump pattern is genuinely useful for Leo: paste unstructured thinking (a brainstorm, a session of design ideas, mid-chapter realizations), command parses it into structured updates across his existing files:
- New insight about a character → append to character bible
- New worldbuilding decision → append to world-bible decision log
- New process improvement → append to user_profile.md
- New project idea → create `~/Desktop/ai-writer/ideas/<slug>.md`

This is post-finals work — requires understanding his exact file structure to route well. But the payoff is high: he never has to manually file his own thoughts.

### Idea 3 — Decision Records as a file pattern

obsidian-mind creates "Decision Record" files for important calls. Leo could adopt the same pattern: when a major project decision is made (e.g., "switching from grunt-mode to creative-mode autonomy on book X"), create a Decision Record file. Reduces re-litigating settled choices (which his user_profile.md says to avoid).

Format would be something like `~/.claude/projects/-Users-leograu/decisions/2026-05-18_jarvis-architecture.md`.

### Idea 4 — Multi-LLM portability matters less than I'd thought

The obsidian-mind cross-LLM design assumes Leo might switch between Claude/Codex/Gemini. He's committed to Claude on Max plan. The portability isn't load-bearing for him. So this design choice doesn't transfer.

## Cleanup

Local clone at `~/Desktop/obsidian-mind` — ~10MB. Safe to delete. Recommend leaving for reference until you build the `/dump` equivalent, then delete.

## Action items (post-finals)

1. **Don't install Obsidian or obsidian-mind.** The memory consolidation goal (one unified memory layer) is better served by your existing stack + a few targeted improvements.
2. **Build `/dump` slash command** that parses freeform paste into structured updates across existing files (project bibles, user_profile, character bibles). This is the highest-value steal from obsidian-mind. Save for week 1 of summer build.
3. **Adopt Decision Record pattern** — start a `~/.claude/decisions/` folder, file major calls as `YYYY-MM-DD_<topic>.md`. Lightweight, no code needed, prevents re-litigating.

---

*Studied 2026-05-18. Recommendation: cherry-pick ideas (especially /dump command), don't install Obsidian.*
