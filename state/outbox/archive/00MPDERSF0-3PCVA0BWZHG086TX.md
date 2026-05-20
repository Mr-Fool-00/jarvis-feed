---
ulid: 00MPDERSF0-3PCVA0BWZHG086TX
slack_channel: D0B4N3MEWJE
slack_thread_ts: 
created_at: 2026-05-20T02:01:10.031389+00:00
status: pending
---
SMS via Twilio it is. Lands in your Messages app as regular green-bubble texts, works whether your Mac is on or not.

Here's the plan — two pieces of work from the conversation:

**1. SMS channel (Twilio)**
- Add `/sms/webhook` route to the CF Worker (Twilio POSTs here when you text)
- Same pipeline as Slack: classify → inbox file (with `source: sms`, `reply_phone: +1...`) → Max processes → outbox
- Extend `processOutbox()` to check source — SMS outbox files get delivered via Twilio API instead of Slack
- Extend `daemon.py` to pass `source`/`reply_phone` fields through inbox→outbox
- New wrangler secrets: `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER`
- No SMS history for MVP (each text is standalone) — can add KV-backed history later

**2. Parallel dispatch (daemon.py)**
- Switch from sequential `for` loop to concurrent dispatch via `asyncio` or `ThreadPoolExecutor`
- Default: parallel (multiple `claude -p` processes at once)
- Sequential override: if a task is flagged as heavy (discovery loop, multi-file work) or if Max is already running 2+ tasks, queue instead of spawning another

**What you'll need to do (once I build it):**
- Sign up at twilio.com (~2 min)
- Buy a phone number ($1/mo)
- Copy Account SID + Auth Token + phone number
- Run 3 `wrangler secret put` commands
- Set the Twilio webhook URL to `https://jarvis-listener.<your-account>.workers.dev/sms/webhook`

Want me to build both now?
