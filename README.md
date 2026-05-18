# jarvis-feed

Personal AI/Claude tooling discovery loop. A scheduled Claude agent runs every 12 hours on Anthropic's cloud, scans Reddit / Hacker News / GitHub / RSS for new high-signal AI/Claude/agent content, ranks against `INTEREST_PROFILE.md`, writes a digest to `digests/`, commits dedupe state in `state/seen.json`, and emails the top 3 via Gmail.

## How it works

1. Anthropic's cron fires `0 0,12 * * *` (= 7pm CDT + 7am CDT)
2. Remote Claude agent (Sonnet 4.6) clones this repo
3. Agent reads `AGENT_RUNBOOK.md` and follows it step by step
4. Agent fetches each source in `SOURCES.yaml`, dedupes against `state/seen.json`
5. Agent ranks new items via `INTEREST_PROFILE.md` and recent `state/feedback.md`
6. Agent writes `digests/<YYYY-MM-DD>_<AM|PM>.md`, commits, pushes
7. Agent emails top 3 items to `grau.enterprises@gmail.com` via Gmail MCP

## Files

| File | Purpose | Edit |
|---|---|---|
| `INTEREST_PROFILE.md` | What counts as "interesting." Ranking prompt references this. | Yes — tune what surfaces |
| `SOURCES.yaml` | What to fetch and how (subs, HN queries, RSS feeds, GitHub topics). | Yes — add/remove sources |
| `AGENT_RUNBOOK.md` | The actual instructions the scheduled agent follows every run. | Yes — to change behavior |
| `state/seen.json` | Dedupe DB. Auto-maintained. | **No** — agent owns this |
| `state/feedback.md` | My reactions to past digests. Agent reads to refine ranking. | Yes — tag items 👍 / 👎 / 🤷 |
| `digests/<date>_<AM\|PM>.md` | Output. One per run. | No — read-only |

## Tuning loop

1. Read the latest digest in `digests/`
2. Append reactions to `state/feedback.md` (👍 / 👎 / 🤷 + one-line note WHY)
3. Commit + push (the agent reads feedback every run, ranking sharpens over weeks)
4. To kill/add a source: edit `SOURCES.yaml`, commit, push

## Manual run

To trigger a one-off run from Claude Code:
```
/schedule
→ Run → pick "jarvis-feed every 12h"
```

## Schedule

Cron: `0 0,12 * * *` (UTC)
- 00:00 UTC = 7:00 PM CDT (evening brief)
- 12:00 UTC = 7:00 AM CDT (morning brief)

Adjust by updating the routine via `/schedule → Update`.
