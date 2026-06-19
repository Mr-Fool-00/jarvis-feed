# CC v2.1.166 fallbackModel setting — 8/10

**Date:** 2026-06-08
**Source URL:** https://github.com/anthropics/claude-code/releases/tag/v2.1.166
**Score:** 8/10
**Category:** Claude Code built-in feature (settings.json config change — not third-party code)

---

## What it is

Claude Code v2.1.166 added a `fallbackModel` setting. You put it in your settings.json and it tells Claude Code: if Opus is overloaded or unavailable, try these models instead, in this order. Up to 3 fallbacks. Works in interactive sessions, background sessions, and AFK runs. Also added: cross-session security hardening — messages from one Claude session can no longer silently request permissions in another session.

---

## Why you'd want it

You run book pipelines overnight and when you're AFK. On June 5th, Opus 4.7/4.8 went down for 6 hours. With the current setup, that's a failed overnight run — no chapter written, wasted budget, you wake up to nothing.

With fallbackModel set to `["claude-sonnet-4-6"]`, Claude Code would have switched to Sonnet when Opus failed and kept writing. The chapters would still have gotten done. The fallback is transparent — you see a note that it switched models, but the task continues.

The fix is literally 3 lines in your `~/.claude/settings.json`:

```json
{
  "model": "claude-opus-4-8",
  "fallbackModel": ["claude-sonnet-4-6", "claude-haiku-4-5"]
}
```

That's it. Done.

---

## Why I think it's worth your attention

This is a 3-minute fix that eliminates an entire failure mode in your pipeline. You've been running without a net. The June 5th outage was real and hit during working hours — at off-peak overnight hours it's rarer but not impossible. Add fallbackModel, stop thinking about it.

The cross-session security hardening also matters for you specifically: as Jarvis grows more complex (multiple CCR sessions running simultaneously), the old behavior where a compromised session could request permissions from another session was a real risk vector. Now it's closed.

---

## What to do

1. Open `~/.claude/settings.json` (or let me do it via a Jarvis skill)
2. Add the `fallbackModel` key with your preferred chain
3. Done — takes effect on next Claude Code startup

If you want, I can build a settings-update skill that applies this and any future config tweaks cleanly.

🔗 https://github.com/anthropics/claude-code/releases/tag/v2.1.166
