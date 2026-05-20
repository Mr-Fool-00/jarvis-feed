---
ulid: EXT-00MPEOGSPB-4ZWCFHDWEEVAW4XX
slack_channel: D0B4N3MEWJE
slack_thread_ts: 
created_at: 2026-05-20T23:13:38.858Z
status: sent
ok: true
---
Every completed task gets appended to a lesson queue file (`~/.claude/skills/_lesson-queue/queue.md`) with its ULID, kind, body/response excerpts, and duration. The `maybe_fire_extractor` gate then checks whether the task deserves immediate extraction — it skips failed tasks, test/discovery kinds, tasks shorter than `EXTRACTOR_MIN_DURATION_S` (default 60s), and responses under `EXTRACTOR_MIN_RESPONSE_CHARS` (default 500 chars). Tasks that pass those heuristics spawn a background daemon thread running `_extractor_worker`, which reads a prompt template from `_lesson-queue/extract_lesson_prompt.md`, concatenates the task body + response + metadata, and pipes it into a second `claude -p --dangerously-skip-permissions` subprocess (capped at 5 minutes). Tasks that don't meet the thresholds still sit in the queue for later batch processing via the `/extract-lessons` slash command.
