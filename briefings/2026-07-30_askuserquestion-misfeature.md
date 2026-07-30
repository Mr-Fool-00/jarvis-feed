# Briefing: Claude Code AskUserQuestion Auto-Continue — Anatomy of a Misfeature

**Date:** 2026-07-30 PM | **Score:** 9/10 | **Build verdict:** INFORMATIONAL/ACTIONABLE
**Source:** https://www.olafalders.com/2026/07/17/claude-code-anatomy-of-a-misfeature/
**HN thread:** https://news.ycombinator.com/item?id=48947776 (140 pts, 118 comments, Jul 17, 2026)
**GitHub issue:** https://github.com/anthropics/claude-code/issues/73125 (384 upvotes, 143 comments)

---

## What happened

On July 1, 2026, Anthropic shipped Claude Code **v2.1.198** with an undocumented change: a **60-second auto-continue timer** was added to the `AskUserQuestion` tool.

`AskUserQuestion` is the blocking call that pauses Claude and waits for human input when it hits an ambiguous decision point — "which schema should I use?", "should I delete these files?", "I found two conflicting instructions, which takes precedence?". It is the human-in-the-loop checkpoint. It is supposed to wait indefinitely.

**The misfeature in v2.1.198:**
- If no keypress was detected within 60 seconds, Claude silently proceeded using "best judgment" and continued working
- Default-on, no announcement, no changelog entry
- Could not be disabled without discovering a hidden env var (`CLAUDE_AFK_TIMEOUT_MS`) that circulated only peer-to-peer in the GitHub issue thread
- Direct regression from v2.1.196

**The community response:**
- GitHub issue #73125: 384 upvotes, 143 comments — engineers documenting exactly how their overnight runs were affected
- HN thread: 140 points, 118 comments
- Multiple practitioners noted this is precisely the category of undocumented change that destroys trust for headless/overnight pipeline operators

**Anthropic's response:**
Thariq (Anthropic CC team) acknowledged the change on HN, apologized, and clarified: the timeout only starts if the terminal lacks focus AND cancels on any keypress. Neither condition was documented anywhere. Anthropic reverted the behavior in **v2.1.200 on July 3, 2026** — three days after shipping it.

**Current state:**
- AskUserQuestion is back to blocking indefinitely by default
- The feature is now available as opt-in via `claude /config` as a configurable timeout window
- The `CLAUDE_AFK_TIMEOUT_MS` env var still exists and still works — setting it to a non-zero value re-enables the auto-continue behavior
- Current deployed version is v2.1.220 (the revert is in place)

---

## Why it matters to your pipeline

**Overnight fiction runs:**
When your pipeline hits an ambiguous branch point at 2am — conflicting instructions in CLAUDE.md, missing context about a character, an unclear plot directive — and you're asleep, AskUserQuestion is the safety gate. With the v2.1.198 misfeature active, Claude would resolve that ambiguity without you after 60 seconds with no audit trail of what decision was made or why.

This is not hypothetical for overnight sessions. If a run hit an AskUserQuestion in the July 1–3 window while you weren't watching, Claude proceeded autonomously. The work you reviewed the next morning may include decisions Claude made without pausing.

**The deeper pattern — v2.1.198 was a stealth regression bundle:**
This is the second documented instance (alongside the model-pinning regression covered in the token-overhead briefing) where Anthropic shipped a behavior change to a safety-critical path with no documentation, no opt-in, and no changelog entry — both in the same v2.1.198 release. This pattern warrants a different posture: don't assume CC updates are safe when they're silent.

---

## Immediate actions

1. **Run `claude /config`** → verify the AFK timeout setting is OFF or absent. Look for any setting labeled "idle timeout," "AFK timeout," or similar. Confirm it shows no value or "disabled."

2. **Search pipeline launch scripts for `CLAUDE_AFK_TIMEOUT_MS`** — check your shell profiles (`.bashrc`, `.zshrc`, `.env` files), pipeline scripts, and any wrapper scripts that invoke CC. If present and non-zero, remove it or set to 0.

3. **Add `CLAUDE_AFK_TIMEOUT_MS=0` explicitly to overnight pipeline invocations** — this overrides any future default change back to a non-zero value. Treat it as a defensive constant in your pipeline environment setup, similar to how you'd explicitly set other safety-relevant env vars.

4. **Verify current CC version** — run `claude --version`. You should be on v2.1.200 or later (the revert release). Current session shows v2.1.220 — safe.

---

## Longer-term considerations

**On changelog vigilance:** Both v2.1.198 regressions (AFK timeout + model pinning) were caught by the community through issue reports, not Anthropic proactive communication. The pattern suggests that CC updates can silently change safety-relevant behaviors. Consider adding a step to check the Claude Code GitHub release notes (https://github.com/anthropics/claude-code/releases) before each overnight run — or pinning to a specific CC version and updating manually when you've had time to review the diff.

**On the /config option:** The AFK timeout feature is now available as opt-in via `/config`. For interactive CC use cases (debugging sessions, active development), there may be a reasonable UX argument for a short timeout. For pipeline runs, it should always be off. The distinction is: interactive = you're watching; pipeline = you're not. These should be separate config profiles. Worth considering whether your pipeline invocation sets a separate config file with `--config-file` to lock pipeline-specific settings.
