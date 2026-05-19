# Two-Way Slack Listener — Architecture Design

**Date:** 2026-05-19 (Tuesday, ~10am CDT)
**Status:** Design only — build deferred to post-finals (after 2026-05-21)
**Goal:** Leo can text in Slack `#all-jarvis` (or DM the Jarvis bot) and get a real Claude response. Today, Slack is one-way push. After this build, it's bidirectional chat.

---

## The gap we're solving

Right now:
```
Anthropic Cloud (Jarvis routine) ──webhook POST──► Slack #all-jarvis
                                                         ▲
                                                         │
Leo types in Slack ──────► [nothing listens] ───X────────┘ (no response possible)
```

After this build:
```
Leo types ──► Slack Events API ──► CF Worker endpoint ──► RemoteTrigger fires "Jarvis chat" routine
                                                                      │
                                          processes via Max budget   │
                                                                      ▼
Slack #all-jarvis ◄──webhook POST──── Routine writes response ──────┘
```

---

## Component breakdown

### 1. Cloudflare Worker (the receiver)

**Why CF Workers, not Mac Mini:**
- Free tier: 100k requests/day, 10ms CPU per request
- Runs on Cloudflare's edge globally (~10-50ms latency to user)
- No server to maintain, no battery to drain
- Custom domain free: `jarvis.<yourname>.workers.dev` or your own domain
- Cost at Leo's expected volume (~50 messages/day max): **$0/month forever**

**Alternative if CF Workers feels too edgy:**
- **Vercel Functions** — similar free tier, slightly different DX
- **Railway / Fly.io** — free tier with a small always-on container if needed
- **GitHub Codespaces** — 60h/month free, overkill for this

Recommendation: **Cloudflare Workers.** Cleanest fit.

**What the Worker does (in pseudocode):**
```js
export default {
  async fetch(request, env, ctx) {
    const body = await request.json();

    // 1. Verify Slack signature (security)
    if (!verifySlackSignature(request, body, env.SLACK_SIGNING_SECRET)) {
      return new Response('Unauthorized', { status: 401 });
    }

    // 2. Handle Slack URL verification challenge (initial setup)
    if (body.type === 'url_verification') {
      return new Response(body.challenge, { status: 200 });
    }

    // 3. For actual events, ack immediately (Slack requires <3s)
    ctx.waitUntil(handleEvent(body, env));
    return new Response('ok', { status: 200 });
  }
};

async function handleEvent(body, env) {
  // Only react to messages mentioning the bot or DMs to it
  const event = body.event;
  if (event.type !== 'app_mention' && event.type !== 'message') return;
  if (event.subtype === 'bot_message') return;  // skip our own posts

  // Write the message context to a state file (via GitHub API)
  await writeToStateFile({
    timestamp: event.ts,
    user: event.user,
    text: event.text,
    channel: event.channel,
    thread_ts: event.thread_ts || event.ts
  }, env.GITHUB_TOKEN);

  // Trigger the Jarvis chat routine on-demand
  await triggerAnthropicRoutine(env.ROUTINE_ID, env.ANTHROPIC_TOKEN);
}
```

### 2. New Anthropic routine: "Jarvis chat"

A SEPARATE routine from the Discovery Loop. Configuration:
- Cron: never (this routine is fired ON-DEMAND only by the CF Worker)
- Schedule: `run_once_at` set to far future (e.g., year 2030) so it's "scheduled" but doesn't auto-fire
- Source: same `Mr-Fool-00/jarvis-feed` repo
- MCP: Gmail attached (for richer responses if needed)
- Prompt: instructs the routine to read `state/inbox/<timestamp>.md`, formulate response, POST to Slack webhook, then clear inbox entry

**Why a separate routine, not the existing Discovery Loop:**
- Discovery Loop has a different job (scheduled feed scan)
- Separation keeps purpose clean — Discovery vs Chat
- Different runtime budget (chat = 1-5 min, not 55)

### 3. Inbox state file pattern

CF Worker writes incoming messages to `state/inbox/<ISO-timestamp>.md` in the repo (via GitHub API). The routine reads any files in that directory, processes each, writes response, deletes the file.

Schema:
```markdown
---
slack_ts: 1747654321.000100
user_id: U01ABCDEF
channel: C01XYZ123
thread_ts: 1747654321.000100
---

<the message text Leo typed>
```

### 4. Response delivery

The chat routine uses the same Slack webhook we already have. Posts response as a threaded reply to the original message (using `thread_ts`).

---

## Setup steps (when Leo's ready, post-finals)

### Phase 1 — Slack side (~15 min)

1. Go to `api.slack.com/apps/<your-app-id>` (the "Jarvis Digest" app from yesterday)
2. Click **"Event Subscriptions"** → toggle ON
3. **Request URL:** `https://jarvis.<yourname>.workers.dev/` (provisional; Slack will verify this — Worker must be deployed first)
4. **Subscribe to bot events:**
   - `app_mention` (responds when @mentioned)
   - `message.im` (DMs to the bot)
5. Click **"OAuth & Permissions"** → add scopes:
   - `chat:write` (already there from webhook)
   - `app_mentions:read`
   - `im:history`, `im:read`, `im:write`
6. Reinstall the app to workspace (scope change requires it)
7. Copy the **Bot User OAuth Token** (`xoxb-...`) — used by the worker

### Phase 2 — Cloudflare Worker (~30 min)

1. Sign up at `cloudflare.com` (free)
2. Install `wrangler` CLI: `npm i -g wrangler`
3. `wrangler login`
4. `wrangler init jarvis-listener` → creates a new Worker project
5. Drop in the pseudocode above (real implementation, not pseudocode)
6. Set secrets via wrangler:
   ```
   wrangler secret put SLACK_SIGNING_SECRET
   wrangler secret put SLACK_BOT_TOKEN
   wrangler secret put GITHUB_TOKEN
   wrangler secret put ANTHROPIC_TRIGGER_TOKEN  # or however Anthropic CCR auth works
   wrangler secret put ROUTINE_ID
   ```
7. `wrangler deploy` → URL becomes `https://jarvis-listener.<account>.workers.dev`
8. Update Slack app's Request URL to this real URL → Slack verifies (Worker's `url_verification` handler responds with challenge)

### Phase 3 — Anthropic routine (~10 min)

1. Use the schedule skill to create a new routine: "Jarvis Chat Listener"
2. `run_once_at` far future (or a different mechanism if Anthropic provides on-demand routines)
3. Source: same repo
4. Prompt: full chat-routine prompt referencing `state/inbox/` directory pattern
5. Add a section to `AGENT_RUNBOOK.md` (or new `CHAT_RUNBOOK.md`) for the chat routine's processing logic

### Phase 4 — Test (~5 min)

1. In Slack `#all-jarvis`, type `@Jarvis hello`
2. Within 30-90 seconds, expect a threaded reply from the bot
3. Iterate if response is wrong

---

## Open questions for Leo (post-finals review)

1. **Bot persona:** Should the chat routine use the full JARVIS_PERSONA.md (heavy, deep), or a lightweight "quick chat" persona for fast back-and-forth?
2. **Latency tolerance:** 30-90 second response time okay, or would on-Mac local Claude Code be preferable for instant chat (at cost of "computer must be on")? Hybrid?
3. **Command-style messages vs free chat:** Should certain prefixes (`/digest`, `/status`, `/dump`) route to specific routines, while plain chat goes to a default conversational one?
4. **Memory across messages:** Should the chat routine read recent Slack history (last N messages in the channel) for context, or treat each message as standalone?
5. **Cost-tracking:** Each chat reply costs Max-plan budget. At ~100 messages/day worst case = ~10% weekly budget. Set a daily cap? Or trust it?

---

## What this enables (the why)

- **You text Jarvis from your phone, get a real response.** This is the entire P3-on-the-cheap.
- **Voice frontend later layers on top.** Whisper → text → Slack → CF Worker → Jarvis → Slack → TTS. All free + Max budget. No Mac needed.
- **Bidirectional means commands work from phone too:** "/dump <thinking>" from Slack works the same as from terminal.
- **The writing-subsystem trigger ("make me a book series") gets a delivery channel.** You type one Slack message, days later Jarvis pings you with the finished book.

This is the unlock for "Jarvis as actual collaborator I can talk to from anywhere."

---

*Architecture designed 2026-05-19. Build deferred to post-finals. Total setup: ~60-90 min when Leo's ready.*
