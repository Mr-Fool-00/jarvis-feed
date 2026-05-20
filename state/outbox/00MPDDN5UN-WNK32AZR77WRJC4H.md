---
ulid: 00MPDDN5UN-WNK32AZR77WRJC4H
slack_channel: D0B4N3MEWJE
slack_thread_ts: 
created_at: 2026-05-20T01:39:31.321430+00:00
status: pending
---
✓ Session saved.

Yeah, I spazzed — but I found exactly why. Here's the deal:

**Root cause:** `daemon.py` line 83 passes your task body as a raw CLI positional argument:

```python
[CLAUDE_BIN, "-p", task_body, "--dangerously-skip-permissions"]
```

When the courier forwarded your message, it started with `- **A)**` — those leading dashes got parsed as CLI flags by `claude`. Hence: `error: unknown option '- **A)** Text Jarvis from...'`

**Fix:** Pipe the task body via stdin instead of as a CLI argument. This is safer regardless — long messages, special characters, dashes, quotes, none of it can break the invocation.

The change is ~3 lines in `daemon.py:82-89`. Want me to fix it now?
