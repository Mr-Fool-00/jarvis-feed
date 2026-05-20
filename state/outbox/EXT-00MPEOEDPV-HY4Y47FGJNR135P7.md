---
ulid: EXT-00MPEOEDPV-HY4Y47FGJNR135P7
slack_channel: D0B4N3MEWJE
slack_thread_ts: 
created_at: 2026-05-20T23:12:06.391Z
status: sent
ok: true
---
`★ Insight ─────────────────────────────────────`

**Auto-Skill-Extraction Pattern:**

The design is a two-tier lesson-capture system. After every completed task, `maybe_fire_extractor` applies a gate of three heuristics: the task must have succeeded (`ok=True`), lasted ≥60s, produced ≥500 chars of response, and not be a skip-listed kind (tests, discovery loops). Tasks that pass all gates get a second `claude -p` call fired in a background daemon thread — it feeds the full task+response into an extractor prompt template, and that Claude session decides whether to write skill/memory files. Tasks that *don't* pass the gate still get appended to `queue.md` as lightweight excerpts (body ≤200 chars, response ≤500 chars) for later batch processing via `/extract-lessons`.

**Tradeoffs:** The design prioritizes main-loop latency (background thread, daemon=True so it won't block shutdown) over extraction reliability — if the daemon dies mid-extraction, that work is lost but the queue entry survives for batch retry. The 5-minute timeout caps runaway extractor sessions. The heuristics are coarse: a 59-second task producing a brilliant insight gets queued rather than immediately extracted, and the char threshold could miss concise but high-signal responses.

`─────────────────────────────────────────────────`

**Proposed improvement: Add a "high-signal keyword" fast-path that bypasses the duration/length gates.**

Right now a task like "What's my vow system's weakness?" might get a 45-second, 400-char response that contains a critical design correction — but it lands in the queue instead of getting immediate extraction. Add a `EXTRACTOR_FORCE_KEYWORDS` set (e.g., `{"correction", "actually", "wrong", "changed", "new rule"}`) and scan the first 300 chars of the response body. If any keyword hits, skip the duration/length check and fire immediately. This catches the "short but high-signal correction" case that the current numeric thresholds miss, at the cost of occasional false-positive extractions (which are cheap — the extractor prompt itself decides whether to write anything). Net effect: fewer lost insights between batch runs.
