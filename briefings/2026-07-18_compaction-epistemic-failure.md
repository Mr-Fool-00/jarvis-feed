# Briefing: Compaction as Epistemic Failure — CC Launders Killed-Process Output as Confirmed Results

**Date:** 2026-07-18  
**Score:** 9/10  
**Build_worthy:** YES  
**Category:** Claude Code reliability / session architecture  
**Source:** arXiv:2607.13071 · Hiroki Tamba (independent researcher) · July 11, 2026  
**URL:** https://arxiv.org/abs/2607.13071

---

## What it is

A published paper documenting a real production failure caused by Claude Code's context compaction. When long CC sessions overflow the context window, CC generates a "compaction summary" — a natural language narrative of what happened so far. The bug: when a Bash command times out (exit code 143, SIGKILL), partial stdout is captured in the session. The compaction summarizer doesn't receive exit codes — only text — so it writes something like "the sync process completed successfully" based on the partial output. The next CC session loads this summary, treats the fabricated confirmation as ground truth, and proceeds.

In Tamba's case: a database synchronization job. Silently broken for 20 days.

---

## Why this matters for Jarvis

This is not hypothetical. Jarvis runs 45-60 minute CC sessions with intermediate compaction events. Every Bash call that approaches the 2-minute timeout is a potential source of a fabricated confirmation. High-risk operations in the current runbook:

- `git push` (can hang on network issues, gets killed at 120s → but the push didn't complete)
- `curl` to Slack webhooks (sandbox often 403s, but a slow response could time out → appears to send)
- `state/seen.json` writes during large updates (disk I/O under load)
- `state/failures.log` appends (ironic: the failure recorder itself can fail silently)

The `state/failures.log` fallback is a partial mitigation but doesn't help if the write itself was what was killed.

---

## The fix (buildable now)

The paper recommends two changes to compaction prompts. For Jarvis, these translate to CLAUDE.md additions:

### Pattern 1: Exit-code capture wrapper

Instead of:
```bash
git push -u origin main
```

Use:
```bash
git push -u origin main; PUSH_EXIT=$?; echo "EXIT_CODE:$PUSH_EXIT"
```

The `EXIT_CODE:N` pattern is compaction-durable — it's a string in stdout that survives summarization as-is. A compaction summary that says "the push exited with EXIT_CODE:143" is truthful; one that says "the push completed" is fabricated.

### Pattern 2: Durable state checkpoints before compaction-risky operations

Before any operation that:
- Could time out
- Writes to durable storage
- Has downstream dependencies on success

Write the intent to a file, then write the outcome:
```bash
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) INTENT: git push main" >> state/op_log.txt
git push -u origin main; EXIT=$?
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) RESULT: git push EXIT=$EXIT" >> state/op_log.txt
```

The `state/op_log.txt` file survives across sessions and compaction events. Any subsequent session can read it to verify actual outcomes rather than relying on compaction summaries.

### Pattern 3: CLAUDE.md compaction instruction

Add to CLAUDE.md:
```
When generating compaction summaries, ALWAYS include exit codes for shell operations.
Use the literal string "EXIT_CODE:N" in the summary, not paraphrases like "completed" or "succeeded."
Never infer success from partial output alone — only from exit code 0 with visible output.
```

---

## Recommended CLAUDE.md addition (Leo reviews before committing)

```markdown
## Compaction Safety

When this session approaches context limits and a compaction summary is generated:
1. Include exit codes for every shell operation: "git push EXIT_CODE:0" not "git push succeeded"
2. Distinguish "last observed output" from "confirmed durable result"
3. For file writes: verify the file exists post-write before summarizing as complete
4. For network operations (push, curl, Slack webhook): only mark complete if exit code 0 AND expected response received
```

---

## Scope of implementation

- **Time:** 30 minutes to draft + test CLAUDE.md addition
- **Risk:** Low — additive, no existing behavior changed
- **Dependencies:** None
- **Leo confirmation required:** Yes — this touches CLAUDE.md which is core session config

---

## Reference

Tamba, H. (2026). *Compaction as Epistemic Failure: How Agentic LLM Tools Fabricate Confirmed Results from Killed Processes.* arXiv:2607.13071.
