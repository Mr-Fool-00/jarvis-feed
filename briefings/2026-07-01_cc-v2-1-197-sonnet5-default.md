# Briefing: CC v2.1.197 — Claude Sonnet 5 Is Now the Default Model

**Date:** 2026-07-01
**Score:** 8/10 — Type A (pipeline configuration, affects every CC session without explicit model override)
**Action:** Pin `defaultModel` in `~/.claude/settings.json` before next overnight fiction run if you want Opus 4.8 behavior. Or test Sonnet 5 on a chapter first.

---

## What changed

Released June 30, 2026 at 17:56 UTC (after the PM Jarvis run). v2.1.197's primary change: **Claude Sonnet 5 replaces Opus 4.8 as the default model in Claude Code**.

Key specs for Sonnet 5 in this context:
- **Native context window: 1,000,000 tokens** (vs. Opus 4.8's 200K) — this is a real capability jump for loading full world-bibles in a single shot
- **Promotional API pricing through August 31, 2026:** $2/Mtok input / $10/Mtok output (standard Sonnet 5 pricing after that)
- **No UI warning on the switch** — if your settings.json doesn't specify `defaultModel`, CC silently switched to Sonnet 5 with this update

---

## Why it matters to your pipelines

Your overnight fiction runs don't specify a `model:` override — they inherit CC's default. As of v2.1.197, that default is now Sonnet 5.

**Upside:** Sonnet 5 is faster than Opus 4.8, and 1M context means you can load an entire novel's world-bible, all character sheets, and the prior 10 chapters without chunking. This is genuinely useful for the chapter-scale generation pipeline.

**Risk:** Sonnet 5 and Opus 4.8 have different capability profiles on the tasks your Council uses — long-horizon consistency, voice matching, adversarial critique. If chapter quality drops or a Council advisor produces different-than-expected output, the model switch is the first variable to check.

**The practical question:** Is Sonnet 5 good enough for your fiction pipeline, or do you want Opus 4.8 explicitly? You don't know yet because you haven't tested it. The 1M context window is a genuine argument for testing Sonnet 5 for pipeline runs that hit context limits.

---

## Fable 5 context

Fable 5 is still offline (Day 20 with no restoration). Before this update, Opus 4.8 was the Max plan ceiling. Now Sonnet 5 is the default instead. This is an unintended second change to your effective model tier — Anthropic has moved the floor and the ceiling simultaneously without an explicit "here's your model stack now" announcement.

Current effective stack (as of July 1, 2026):
- **CC default:** Sonnet 5 (new)
- **Explicit upgrade:** Opus 4.8 (still available on Max)
- **Top tier (unavailable):** Fable 5 (banned)

---

## What to do

**Option A — Test Sonnet 5 first:**
Run a single chapter through the pipeline without any model override. Compare output to the last Opus 4.8 chapter for voice consistency, structural quality, adversarial critique sharpness. If it passes, Sonnet 5 is probably fine and the 1M context is a net win.

**Option B — Pin Opus 4.8 immediately:**
Add to `~/.claude/settings.json`:
```json
{
  "defaultModel": "claude-opus-4-8"
}
```
This reverts CC's default to Opus 4.8 until you explicitly test Sonnet 5.

**Option C — Upgrade to v2.1.197 and check:**
If you haven't upgraded yet:
```bash
npm update -g @anthropic-ai/claude-code
```
Then check current model via `claude --version` or look at the session header.

Recommendation: **Option A before next overnight run.** Sonnet 5's 1M context is a meaningful pipeline capability, and if quality is comparable, staying on the default is simpler than managing overrides.

---

## Sources

- github.com/anthropics/claude-code/releases/tag/v2.1.197 — official release notes
- Anthropic changelog — model capabilities specification for Sonnet 5

---

*React 🧪 to kick off a Sonnet 5 vs Opus 4.8 chapter quality comparison run.*
*React 📌 if you want to pin Opus 4.8 explicitly before any testing.*
