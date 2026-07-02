# Briefing: CC PostSession Hook — Session-End Snapshot for Overnight Runs

**Date:** 2026-07-02
**Score:** 8/10 · **Build verdict:** BUILD-WORTHY
**Source:** Claude Code v2.1.198 release notes · github.com/anthropics/claude-code/releases/tag/v2.1.198

---

## What it is

The `postSession` lifecycle hook in CC v2.1.198 fires a shell script after a Claude Code session ends cleanly — whether the session exits via `/quit`, times out, or completes in background/headless mode. You configure it in `.claude/settings.json`:

```json
{
  "hooks": {
    "postSession": {
      "command": "/path/to/your/script.sh"
    }
  }
}
```

The hook receives session metadata as environment variables (session ID, exit reason, timestamp). It runs in the project's working directory.

---

## Why you'd want it

Right now when an overnight fiction run ends, there's no signal. The session either completed its chapter or died somewhere in the middle — you find out in the morning by looking at git. If CC crashed without committing, unfinished work is gone.

PostSession hook changes that:

- **Auto-commit on exit:** The hook can `git add -A && git commit -m "session-end: $(date) — $(cat .claude/last_chapter.txt)"` — any open chapter files get committed at session end, not just when the agent decides to commit.
- **Session summary export:** Write out a summary of what the session accomplished to a file or a Slack webhook — no more "did it finish or just die?" ambiguity.
- **Heartbeat write:** Touch a file with the exit timestamp so the next run knows when the previous one ended.

---

## Why I think it matters

Leo's overnight fiction runs are the highest-risk, lowest-visibility part of the pipeline. They run for hours with nobody watching, and right now there's no guaranteed snapshot at the end. The PostSession hook is the first native mechanism CC has offered for "do something when the session finishes" — it's purpose-built for exactly this use case.

The gap it fills: today, if an overnight run gets 80% through a chapter and CC's session times out, the partial chapter either got committed (if the agent happened to commit) or it didn't (if it didn't). PostSession ensures the commit happens regardless.

---

## What to do

**Suggested implementation for the fiction pipeline:**

1. Create `scripts/postsession.sh`:
   ```bash
   #!/usr/bin/env bash
   # Runs after every CC session ends — commit any open chapter work
   set -e
   cd "$CLAUDE_PROJECT_DIR"
   
   if git diff --quiet && git diff --staged --quiet; then
     echo "postsession: nothing to commit" >&2
     exit 0
   fi
   
   CHAPTER=$(cat .claude/current_chapter.txt 2>/dev/null || echo "unknown")
   git add -A
   git commit -m "session-end: chapter ${CHAPTER} snapshot ($(date -u +%Y-%m-%dT%H:%M:%SZ))"
   git push origin main
   echo "postsession: snapshot committed and pushed" >&2
   ```

2. Wire it in `.claude/settings.json`:
   ```json
   {
     "hooks": {
       "postSession": {
         "command": "bash scripts/postsession.sh"
       }
     }
   }
   ```

3. Have the fiction agent write the current chapter number to `.claude/current_chapter.txt` at session start, so the snapshot commit message is meaningful.

**Estimated effort:** 30–45 minutes to implement and test on a short fiction session.

**Caveats:**
- Hook only fires on *clean* exits. If CC crashes hard (OOM, SIGKILL, container kill), the hook may not run — so this doesn't replace in-session commits, it supplements them.
- Shell script runs with the session's environment, including any secrets loaded by the shell profile. Keep the script minimal.
- Requires CC v2.1.198+. Confirm CCR is running this version before relying on the hook.

---

## Related CC v2.1.198 features (surfaced same release, less urgent)

- **`/dataviz` skill:** Native design system for charts/dashboards. Not directly fiction-pipeline relevant but handy for Jarvis digest stats.
- **`/cd` command:** Change working directory mid-session without rebuilding prompt cache.
- **`--safe-mode` / `CLAUDE_CODE_SAFE_MODE`:** Launch CC with all customizations disabled (CLAUDE.md, plugins, skills, hooks, MCPs). Use for clean-room troubleshooting when a session is broken.
- **`disableBundledSkills`:** Hide built-in slash commands from the skill list — reduces noise when project skills should be the only thing showing.
