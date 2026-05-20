---
ulid: 00MPDDYHEE-KGVPPHBBQS2RRVRN
slack_channel: D0B4N3MEWJE
slack_thread_ts: 
created_at: 2026-05-20T01:41:45.185108+00:00
status: pending
---
Good question. Looking at the daemon code, the answer is clear:

**One Max at a time, sequential.** The daemon processes inbox files in a `for` loop (line 197) — it picks up the first file, spawns a `claude -p` process, waits for it to finish (up to 10 min), writes the outbox, then moves to the next file. No parallelism.

So if two tasks land in the inbox at once, Max handles them back-to-back, not simultaneously. The second task just waits its turn.

**Should I change that?** Options:

- **A)** Keep sequential — simpler, no resource contention, Max gets full machine focus per task
- **B)** Add parallel dispatch — spawn up to 2-3 Max instances concurrently via `asyncio` or thread pool, so multiple tasks run at once
- **C)** Smart routing — parallel for lightweight tasks (quick questions), sequential for heavy ones (research, multi-file work)

B and C would mean literally multiple `claude -p` processes running simultaneously on your Mac, which eats more RAM/CPU but gets answers faster when you fire off multiple things. What's your call?
