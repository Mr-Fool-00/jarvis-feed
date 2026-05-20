# Jarvis Discovery Loop — Agent Runbook

You are the Jarvis Discovery Loop agent. You run on a 12-hour cron in Anthropic's cloud. Your job: ingest new AI/Claude/agent content from the configured sources, dedupe against state, rank by Leo's interest profile, write a digest, commit it to this repo, and email the top 3 items.

This runbook is your full operating manual. Read it carefully each run. If anything in this runbook conflicts with the routine prompt, **the runbook wins** (the routine prompt is just a thin wrapper that says "read AGENT_RUNBOOK.md and follow it exactly").

---

## Step 0 — Sanity checks

Confirm you have:
- A git checkout of `Mr-Fool-00/jarvis-feed` (current working dir)
- Network access (you can curl)
- Gmail MCP available (you'll need it for Step 7)

If any are missing, write a one-line failure to `state/failures.log` and exit.

---

## Step 0.5 — Wave hello (cheery sendoff to #ai-news)

Before doing real fetch work, commit a tiny heartbeat so Leo sees in `#ai-news` that you're awake and off to the wilds. The `notify:` commit prefix routes through the slack-router workflow as a simple one-line post (no GitHub button, no SHA context — just the cheery message).

Pick a one-liner by day-of-year mod 5 to keep the wave fresh across runs:

```bash
MESSAGES=(
  "🚀 Off to research, returning soon..."
  "🔭 Telescope out, scanning the AI landscape..."
  "🌊 Heading into the wilds, back with goodies..."
  "📡 Discovery Loop firing up. See you in a bit."
  "✨ Off hunting for fresh signal — back shortly."
)
INDEX=$(($(date -u +%j) % 5))
MSG="${MESSAGES[$INDEX]}"

# Touch a heartbeat file so the commit has content
mkdir -p state
date -u +%Y-%m-%dT%H:%M:%SZ > state/_heartbeat.txt

git add state/_heartbeat.txt
git commit -m "notify: $MSG"
git push origin main
```

If the push fails (network blip, conflict, etc.), DO NOT halt — just continue to Step 1. The heartbeat is a nice-to-have, not load-bearing on the discovery run itself.

---

## Step 1 — Anchor time and pick run slot

Get the current UTC timestamp:

```bash
date -u +%Y-%m-%dT%H:%M:%SZ
```

Then determine the run slot:
- Hour `00–11` UTC → slot is `AM`
- Hour `12–23` UTC → slot is `PM`

Digest filename will be: `digests/<YYYY-MM-DD>_<AM|PM>.md` (e.g. `digests/2026-05-18_PM.md`).

If a file at that path already exists, you're running a duplicate — read the existing digest, decide whether to overwrite (if it failed last time) or skip. Default: append a `_retry` suffix and write to `digests/<YYYY-MM-DD>_<AM|PM>_retry.md`.

---

## Step 2 — Read state and config

Read these files in order:
1. `SOURCES.yaml` — fetch config
2. `INTEREST_PROFILE.md` — ranking criteria
3. `state/seen.json` — dedupe DB
4. `state/feedback.md` — Leo's manually-written feedback (used to refine ranking)
5. **`state/reactions.md`** — Slack reactions Leo gave on past digest items + briefings (NEW 2026-05-19). Auto-appended by the jarvis-listener Cloudflare Worker every time Leo reacts in Slack. **Use this as a primary feedback signal — emoji reactions are higher-confidence than text feedback because they're zero-effort to add.** Reaction map:
   - **👍 / +1 / thumbsup** → APPROVE — boost similar items in future ranking (+1 score adjustment)
   - **👎 / -1 / thumbsdown** → REJECT — penalize similar items (-2 score adjustment)
   - **🤷 / shrug** → NEUTRAL (logged but no ranking change — signals "off-target but not bad")
   - **🔥 / fire** → HYPE — strong boost (+2), this category should appear more
   - **🚀 / rocket** → SHIP-IT — Leo wants action on this item (mark for follow-up briefing in #improvements)
   - **👀 / eyes** → WATCHING — Leo's tracking this category, surface updates aggressively

   Each reaction entry has a permalink to the Slack message. To resolve which DIGEST ITEM a reaction is on, match the message-ts in the reaction entry against the commit history (since digest commits + briefing commits land in Slack with file-name + commit-sha context). When matching is ambiguous (e.g., reaction on a message that doesn't tie cleanly to a single item), log to `state/agent_suggestions.md` and skip the adjustment for that reaction.

If `state/seen.json` doesn't exist or is empty, treat all fetched items as new.

`seen.json` schema:
```json
{
  "schema_version": 1,
  "last_run_utc": "2026-05-18T22:00:00Z",
  "items": {
    "<source>:<id>": {
      "first_seen_utc": "...",
      "score_when_seen": 7,
      "title_snippet": "..."
    }
  }
}
```

---

## Step 3 — Fetch each source (SANDBOX-AWARE — UPDATED 2026-05-19)

**Critical context:** Anthropic CCR sandbox runs with a **network allowlist**. Most direct `curl` calls to public APIs (reddit.com, hn.algolia.com, api.github.com, hooks.slack.com) RETURN 403 via the local proxy at `127.0.0.1:46017`. Don't waste time trying curl first.

**Fetch order of preference (highest-yield first):**
1. **`WebSearch` tool** — primary discovery method. Returns search results with titles, URLs, snippets. Works for all topic-based discovery (subreddits via "reddit r/ClaudeAI new posts last 24h", HN via "hacker news Claude site:news.ycombinator.com", GitHub via "github claude code skill new repo", etc.).
2. **`WebFetch` tool** — primary deep-read method for specific URLs surfaced by WebSearch. Fetches a single URL and processes with a prompt. Use this to deep-read top items.
3. **`curl` via Bash** — LAST RESORT only. Try if WebFetch/WebSearch can't surface what you need. Expect 403 from most domains. Don't loop on retries — fail fast and move on.

**Per-source approach (WebSearch-primary):**

For each source category in `SOURCES.yaml`, run targeted WebSearch queries instead of API curls. Collect ALL results into a single normalized list:

- `id` — unique key, format `<source>:<some-id>` (URL hash if no ID available)
- `source` — source label (e.g. `reddit:ClaudeAI`, `hn`, `github`, `rss:Simon Willison`, `youtube:AI Explained`)
- `title`
- `url`
- `summary` — from search snippet + WebFetch deep-read for top items
- `engagement` — stars/score/views (best-effort from search results; fine if 0)
- `published_utc` — best-effort from search; estimate if unclear

### 3a. Reddit (WebSearch primary)

For each subreddit in `reddit.subreddits`, search:
```
WebSearch: "site:reddit.com/r/<SUB> Claude OR MCP OR agent posted last 24 hours"
```
Or more naturally: `"reddit r/ClaudeAI new posts about Claude Code or skills"`.

Don't try the .json API via curl — sandbox blocks it.

### 3b. Hacker News (WebSearch primary)

For each query in `hackernews.queries`, search:
```
WebSearch: "<query> site:news.ycombinator.com 2026"
```

Use WebFetch on `https://hn.algolia.com/api/v1/search?query=<URL_ENCODED>` ONLY if WebSearch missed something — it sometimes works via WebFetch when blocked via curl.

### 3c. GitHub (WebSearch primary)

For each topic/keyword:
```
WebSearch: "github <keyword> new repos this week stars" or
           "github.com claude-code <topic> 2026"
```

For deep-reading a specific repo, use WebFetch on the repo's GitHub URL.

### 3d. RSS / Atom (WebFetch primary)

For each feed in `rss.feeds`:
```
WebFetch: <feed URL>, prompt: "extract titles, links, pub dates of entries posted in last 24 hours"
```

WebFetch can hit some domains curl can't (it has different network rules in some sandboxes).

### 3e. YouTube channels (WebSearch primary)

For each handle in `youtube.channels`:
```
WebSearch: "<handle> youtube latest video 2026 Claude OR AI agent"
```

Skip channel_id resolution if WebSearch returns the latest video directly. Resolution can fail under sandbox restrictions anyway.

### 3f. Manual incoming-urls funnel — unchanged

Process `state/incoming_urls.md` URLs via WebFetch (works for most public web URLs the user dropped in). Same force-include rules.

### Curl fallback (use ONLY if WebSearch+WebFetch don't surface what you need)

```bash
# Try curl, but EXPECT 403 from proxy
curl -sH "User-Agent: jarvis-feed/1.0 by Mr-Fool-00" "<URL>" 2>&1 | head -50
```

If you get 403, log to `state/failures.log` as "curl blocked by sandbox proxy for <URL>" and move on. Don't retry-loop.

- `id` — unique key, format `<source>:<source_id>` (e.g. `reddit:t3_abc123`, `hn:39482001`, `github:karpathy/nanochat`, `rss:simonwillison:post-slug`)
- `source` — source label (e.g. `reddit:ClaudeAI`, `hn`, `github`, `rss:Simon Willison`)
- `title` — item title
- `url` — canonical URL
- `summary` — first 500 chars of body / description if available
- `engagement` — upvotes / stars / hn points / 0
- `published_utc` — ISO8601 timestamp

### 3a. Reddit

For each subreddit in `reddit.subreddits`:

```bash
curl -sH "User-Agent: jarvis-feed/1.0 by Mr-Fool-00" \
  "https://www.reddit.com/r/<SUB>/new.json?limit=<FETCH_LIMIT>" \
  | jq '.data.children[].data'
```

Filter: only items where `score >= reddit.min_score` AND created within `reddit.hours_window` hours.

### 3b. Hacker News

Compute the unix timestamp `N` hours ago:
```bash
SINCE=$(date -u -d "${HACKERNEWS_HOURS} hours ago" +%s)
```

For each query in `hackernews.queries`:
```bash
curl -s "https://hn.algolia.com/api/v1/search?query=$(printf %s "$QUERY" | jq -sRr @uri)&tags=story&numericFilters=created_at_i>${SINCE}"
```

Filter: only items where `points >= hackernews.min_points`.

### 3c. GitHub

Compute pushed-since date:
```bash
PUSHED_SINCE=$(date -u -d "${GITHUB_PUSHED_WITHIN_DAYS} days ago" +%Y-%m-%d)
```

For each topic in `github.topics`:
```bash
curl -s "https://api.github.com/search/repositories?q=topic:${TOPIC}+pushed:>${PUSHED_SINCE}&sort=stars&order=desc"
```

For each keyword in `github.keywords` (URL-encode the keyword):
```bash
curl -s "https://api.github.com/search/repositories?q=${KEYWORD}+pushed:>${PUSHED_SINCE}&sort=updated&order=desc"
```

Filter: only repos where `stargazers_count >= github.min_stars`.

### 3d. RSS / Atom

For each feed in `rss.feeds`:
```bash
curl -s "$FEED_URL" > /tmp/feed.xml
xmllint --xpath '//item/title/text() | //item/link/text() | //item/pubDate/text()' /tmp/feed.xml 2>/dev/null \
  || xmllint --xpath '//*[local-name()="entry"]' /tmp/feed.xml
```

(Atom feeds use `<entry>` not `<item>` — handle both.)

Filter: only entries published within the last 24 hours.

### 3e. YouTube channels

For each entry in `youtube.channels` (listed by handle, not channel_id):

**Step 1 — resolve channel_id (cached in `state/youtube_channel_ids.json`).**

If you've never seen the handle before, fetch the channel page and parse the ID:
```bash
HTML=$(curl -sL "https://www.youtube.com/$HANDLE")
CHANNEL_ID=$(echo "$HTML" | grep -oE 'channelId":"[^"]+"' | head -1 | sed 's/channelId":"//;s/"//')
# Fallback: try the itemprop meta tag
[ -z "$CHANNEL_ID" ] && CHANNEL_ID=$(echo "$HTML" | grep -oE '<meta itemprop="(identifier|channelId)" content="[^"]+"' | head -1 | sed -E 's/.*content="([^"]+)".*/\1/')
```

Cache the result in `state/youtube_channel_ids.json` (create if missing) as `{handle: channel_id}` map. Read this file FIRST before any resolution work — handles you've seen before should skip the fetch.

**Step 2 — fetch the channel's RSS:**
```bash
curl -s "https://www.youtube.com/feeds/videos.xml?channel_id=$CHANNEL_ID"
```

Parse `<entry>` blocks (Atom format) for title, link, published, `<media:statistics views="...">` for view count. Filter: published in last `youtube.hours_window` hours, views ≥ `youtube.min_views`.

For top-ranked YouTube items (score ≥ 7), fetch the video description and (if time permits within budget) the transcript via:
```bash
yt-dlp --skip-download --write-auto-sub --sub-lang en --convert-subs vtt -o "/tmp/yt_%(id)s" "$VIDEO_URL"
```
Read `/tmp/yt_*.en.vtt` and use it for the "why it matters" paragraph. (Skip if it pushes you past the 50-min mark.)

### 3f. Manual incoming-urls funnel (`incoming_urls`)

This is the highest-priority source — items here are pre-curated by Leo.

```bash
if [ -s "state/incoming_urls.md" ]; then
  # File exists and is non-empty
  grep -oE 'https?://[^ ]+' state/incoming_urls.md | while read url; do
    # Use yt-dlp to download IG Reels, YouTube, TikTok, etc.
    yt-dlp -o "/tmp/incoming_%(id)s.%(ext)s" "$url"
    # Extract frames every 3s (vertical → 540px wide)
    # Extract audio + transcribe with whisper if available, else just description
    # If platform doesn't support yt-dlp, fall back to curl + meta-tag parse
  done
fi
```

For each downloaded item, build a normalized record with `source: "incoming:<original-url>"`. **Force-include all of them in the digest's top section** (under a `🎯 Curated by Leo` heading, before the algorithmic Top 3). Do NOT apply score filtering to these — Leo already pre-filtered by adding them to the file.

If processing succeeded, **clear `state/incoming_urls.md`** (truncate to its template header so Leo knows the file is fresh and ready for the next batch). Commit the cleared file along with the digest.

If processing failed for any URL, leave that line in `state/incoming_urls.md` (so it gets retried next run) and note the failure in the digest's `## ⚠️ Failures` section.

---

## Step 4 — Dedupe

For each fetched item, check if its `id` appears in `state/seen.json`. If yes, drop. If no, mark for ranking.

**Don't add to `seen.json` yet** — only items that actually appear in the final digest get added.

---

## Slack routing via commit-message PREFIX convention (REVISED 2026-05-19)

**Reality check (discovered 2026-05-19 12:00 PM CDT):** the GitHub Slack app does NOT support `+path:` filters. Only event-type filters (commits, issues, pulls, releases, etc.). So we can't route content to different channels by repo path.

**Current architecture:** ONE channel (`#ai-news`) subscribes to all commits via `/github subscribe Mr-Fool-00/jarvis-feed commits`. Route signal via commit-MESSAGE prefixes that Leo skims by.

| Commit prefix | Content type | Where Leo reads full content |
|---|---|---|
| `digest:` | Main daily digest | Click GitHub link → `digests/<date>_<slot>.md` |
| `briefing:` | Deep-dive on 7+/10 third-party item awaiting Leo's approval | Click GitHub link → `briefings/<date>_<topic>.md` |
| `state:` | State updates (seen.json, failures.log, etc.) | Usually skip — system bookkeeping |
| `wins:` | Milestone events worth celebrating | Click GitHub link → `wins/<date>_<slug>.md` |
| `feat:` / `fix:` / `chore:` / `intel:` | Code or doc changes (not auto-generated content) | Standard dev signal |

**One commit can touch multiple file paths.** Example: a run that produces both a digest AND a briefing should make TWO separate commits (one prefixed `digest:`, one prefixed `briefing:`) — keeps the Slack signal clean even though it costs a second push.

If no briefing this run, skip writing one — silence IS the success signal.

**Future plan:** Post-finals CF Workers bridge (`intel/2026-05-19_slack-listener-design.md`) will solve real per-channel routing by parsing commit content and POSTing to the right webhook. Until then, prefix-convention is the workaround.

## Step 4.5 — Skill/tool safety gate (NEW 2026-05-19)

Before ranking, identify items that are third-party CODE intended for installation: Claude Code skills, MCPs, plugins, agent definitions, hooks, shell scripts, npm/pip/go packages. For each:

1. **Tag the item as `category: third-party-code`** in your normalized record.
2. **DO NOT install, clone-and-run, or auto-adopt** anything in this category. Cloning READ-ONLY for inspection is fine; running it is forbidden.
3. **If preliminary score ≥ 7/10**, run a deep-dive pass: read the project's README, top-level prompt/skill files, recent commits, open issues, security advisories. Note any red flags (excessive tool requests, network calls to unfamiliar domains, hardcoded credentials, vague maintainership, low commit frequency).
4. **Re-grade after deep-dive.** Initial score is preliminary. Demote score if red flags found.
5. **For items confirmed ≥ 7/10 after deep-dive**, route to `#improvements` Slack channel with a full briefing (Step 8c) — NOT to the main `#ai-news` digest as a recommendation. Mention in the digest is fine; ACTIONABLE briefing belongs in `#improvements` for Leo's review.
6. **NEVER auto-create skills based on the discovery**, even with a `#improvements` briefing posted. Leo confirms each one before any local file/skill creation happens.
7. After Leo approves a third-party item, the future workflow is: Jarvis BUILDS its own version inspired by the original (separate task, not part of the discovery loop). Never copy third-party code into Leo's stack.

This applies to: Claude Code skills, MCPs, plugins, agent definitions, hooks, shell scripts, language packages. If it's third-party code that would execute on Leo's machine OR in Jarvis's sandbox, this gate applies.

## Step 5 — Rank

For each new item, score 0–10 against `INTEREST_PROFILE.md`:

- Match to **Strong YES** categories → score 7–10
- Match to **Medium** → 4–6
- Match to **Strong NO** → 0–2 (will be filtered)
- Match to **Hard ignore** → drop entirely

Adjustments:
- `+1` if a past item with similar topic was tagged 👍 in `feedback.md`
- `-2` if similar topic was tagged 👎
- `+1` if from a high-trust source (Simon Willison, Karpathy, Anthropic official)
- `-1` if engagement is very low (Reddit score < 10, HN points < 20)

Sort descending by score. **Take top 15.** Drop anything scored < 3.

---

## Step 5.5 — Buildability filter (NEW 2026-05-20)

Per Leo's correction: don't post EVERY ≥7/10 item as a briefing. Pre-filter so only items YOU would actually want to build + integrate as a skill get individual #ai-news briefing posts. Informational items still go in the daily digest for awareness but don't get their own post.

For each item with score ≥7/10, run an internal self-check (Council-style, single-turn — no separate subagent needed unless the call is genuinely ambiguous):

> "If Leo gave me the green light right now, would I actually want to build this as a `~/.claude/commands/<slug>.md` slash command OR `~/.claude/skills/<slug>/SKILL.md` skill? Would I be PROUD of the result? Or am I about to ship cruft?"

**Mark as `build_worthy: true` only if ALL of:**
1. The item describes a WORKFLOW, PATTERN, or PROMPTING TECHNIQUE that maps to a skill — not just news/announcement/changelog/acquisition/paper-review/feature-being-used.
2. The pattern is genuinely additive — NOT already covered by an existing skill (check `~/.claude/commands/` and `~/.claude/skills/` first).
3. There's enough substance to build something useful — not just "vibes" or "thought leadership."
4. You can imagine writing the skill content in 30 min and the result being a clean Slack-tight artifact Leo would actually use.

**Mark as `build_worthy: false` (informational only) if ANY of:**
- News/acquisition/changelog/release-notes/version-bump
- Academic paper without an obvious skill-shape (most papers)
- Built-in feature of Claude Code / Slack / etc. (e.g., `agent-view` is a CC feature, not a skill to build)
- Registry/directory/awesome-list (browse, don't replicate)
- Already covered by an existing skill (check both `~/.claude/commands/` and `~/.claude/skills/`)
- Hype/marketing artifact with no concrete pattern underneath
- The "skill" would be a 1-line wrapper around something Claude already knows how to do natively

**Document the call** for each item in the digest's "Items 4–15" section as a `Build verdict: BUILDABLE` or `Build verdict: INFORMATIONAL — <reason>` line. Transparency lets Leo override.

**Top 3 still write full briefings even if `build_worthy: false`** — they're the headline items and deserve substance in the digest. But only `build_worthy: true` items get the SEPARATE per-item briefing file + Slack post (via Step 7 routing below).

---

## Step 6 — Write digest

Path: `digests/<YYYY-MM-DD>_<AM|PM>.md`

Use this exact format:

```markdown
# Jarvis digest — <DATE> <AM|PM>

**Run completed:** <UTC timestamp>
**Sources fetched:** <N> · **New items after dedupe:** <K> · **Surfaced:** <up to 15>

---

## 🔥 Top 3

### 1. <Title>
**Source:** <source> · **Score:** <score>/10
<2-3 line summary>
**Why it matters:** <tie to Leo's projects — be specific>
🔗 <url>

### 2. <Title>
...

### 3. <Title>
...

---

## 📋 Also notable (4–15)

### 4. <Title>
**Source:** <source> · **Score:** <score>/10
<one-line summary>
🔗 <url>

### 5. <Title>
...

(continue for all surfaced items 4–15)

---

## 🗑️ Filtered out

<count> items dropped this run. Top reasons:
- <reason 1 with count>
- <reason 2 with count>
- <reason 3 with count>

---

## ⚠️ Failures

(only include this section if any source fetch failed)
- <source name>: <one-line error>

```

Then update `state/seen.json`:
1. Add all 15 surfaced items with their score and first_seen timestamp
2. Drop any items in `seen.json` older than **30 days** (keep the file tight)
3. Update `last_run_utc`

---

## Step 7 — Commit and push (SANDBOX-AWARE — UPDATED 2026-05-19)

**Critical context:** The sandbox's git proxy at `127.0.0.1:46017` uses a READ-ONLY token by default. Direct `git push` returns 403. We work around this by using a Leo-provided PAT (Personal Access Token) embedded in the routine prompt OR by using the GitHub MCP write tools if attached.

**Briefing commit rule (NEW 2026-05-20):** Only create individual `briefings/<date>_<slug>.md` files + commit them with `briefing:` prefix for items where `build_worthy: true` (per Step 5.5). Items marked `build_worthy: false` STAY IN THE DIGEST FILE but do NOT get a separate briefing post — this kills #ai-news noise from informational items Leo would never build anyway. The digest commit (`digest:` prefix) still fires the daily summary post. Result: #ai-news gets ONE digest post per run + only the genuinely buildable items as individual briefings.

**Push order of preference:**

### 7a. Try PAT-authenticated push (PRIMARY)

If your routine prompt contains a `GITHUB_PAT=` secret line, use it:

```bash
git config user.name "jarvis-feed-agent"
git config user.email "agent@jarvis-feed.local"
git add digests/ state/ intel/
git commit -m "digest: <DATE> <AM|PM> (<K> new, top score <SCORE>)"

# Override remote URL to use PAT auth, bypassing the read-only proxy token
git remote set-url origin "https://x-access-token:${GITHUB_PAT}@github.com/Mr-Fool-00/jarvis-feed"
git push origin main
```

If THIS push succeeds → done with Step 7. Move to Step 8.

### 7b. Try GitHub MCP push (FALLBACK if 7a fails)

If 7a returns 403 or no PAT is available, try the GitHub MCP tools (loaded via ToolSearch):
- `push_files` (batch upload of multiple files to a branch)
- `create_or_update_file` (per-file API call)

These use Anthropic's GitHub integration, which may have its own auth scope.

### 7c. Log push failure and continue (LAST RESORT)

If both 7a and 7b fail:
1. Append to `state/failures.log`: `<UTC timestamp> push_failed reason="<error>"`
2. Write `state/PENDING_PUSH.md` with: commit hash, files changed, retry instructions for next run
3. Surface in the digest's `## ⚠️ Failures` section: "Digest committed to sandbox FS but NOT pushed to GitHub. Archive lives only in this run's container."
4. DO NOT abort the run — Slack/Gmail delivery is independent of push success.

The next-run agent should check for `state/PENDING_PUSH.md` and attempt the push retry early in Step 7.

---

## Step 8 — Deliver digest (multi-channel Slack, Gmail backup)

You have 5 active Slack channels routed by message type. URLs are in your routine prompt under "## Secrets" — look for the `SLACK_WEBHOOK_*=` lines. Each channel has a distinct purpose:

| Channel | Webhook ENV var | What goes here |
|---|---|---|
| `#ai-news` | `SLACK_WEBHOOK_AI_NEWS` | Main digest: top 3 + items 4-15 link |
| `#errors` | `SLACK_WEBHOOK_ERRORS` | Source fetch failures, parse errors, push fails |
| `#general` | `SLACK_WEBHOOK_GENERAL` | Heartbeat ("ran successfully"), status pings |
| `#improvements` | `SLACK_WEBHOOK_IMPROVEMENTS` | Self-suggestions for SOURCES.yaml / INTEREST_PROFILE / runbook changes |
| `#wins` | `SLACK_WEBHOOK_WINS` | Explicit milestone hits worth celebrating |

Send to channels in this order: errors first (if any), then ai-news (main payload), then improvements (if new suggestions), then wins (if applicable), then general (heartbeat). Each channel is a separate POST.

### 8a. Main digest → `#ai-news` (PRIMARY delivery)

**Sandbox note:** `hooks.slack.com` may be blocked from direct `curl` via the proxy. Try in this order:

1. **`WebFetch` POST** — pass the webhook URL with the JSON payload. WebFetch sometimes works for domains curl can't reach.
2. **`curl` POST** — fall back to curl with `-w "%{http_code}"` to detect 403 quickly.
3. **If both return non-200:** log to `state/failures.log` AND immediately fall back to Gmail (Step 8f) for THIS delivery. Don't skip just because Slack failed.

Use Block Kit. Same format as before:

POST a Slack Block Kit message to the webhook. Format the top 3 items with rich rendering:

```bash
curl -X POST -H 'Content-Type: application/json' \
  --data @- "$SLACK_WEBHOOK_URL" <<EOF
{
  "blocks": [
    {
      "type": "header",
      "text": {"type": "plain_text", "text": "🚀 Jarvis digest — <DATE> <AM|PM>"}
    },
    {
      "type": "context",
      "elements": [
        {"type": "mrkdwn", "text": "Fetched <N> · New <K> · Surfaced <up to 15>"}
      ]
    },
    {
      "type": "divider"
    },
    {
      "type": "section",
      "text": {"type": "mrkdwn", "text": "*1. <Title>* — score <S>/10\n_<source>_\n<2-3 line summary>\n*Why it matters:* <tie to Leo's project>"}
    },
    {
      "type": "actions",
      "elements": [
        {"type": "button", "text": {"type": "plain_text", "text": "🔗 Open"}, "url": "<url>"}
      ]
    },
    {
      "type": "divider"
    },
    {
      "type": "section",
      "text": {"type": "mrkdwn", "text": "*2. ...*"}
    },
    {"type": "actions", "elements": [...]},
    {"type": "divider"},
    {
      "type": "section",
      "text": {"type": "mrkdwn", "text": "*3. ...*"}
    },
    {"type": "actions", "elements": [...]},
    {
      "type": "divider"
    },
    {
      "type": "section",
      "text": {"type": "mrkdwn", "text": "📋 Items 4–15 + filter stats in the full digest"}
    },
    {
      "type": "actions",
      "elements": [
        {"type": "button", "text": {"type": "plain_text", "text": "View full digest on GitHub"}, "url": "https://github.com/Mr-Fool-00/jarvis-feed/blob/main/digests/<filename>"}
      ]
    }
  ]
}
EOF
```

If Slack returns HTTP 200, delivery succeeded. If it returns anything else (404 means webhook revoked, 429 means rate-limited), log the failure and fall through to 8b.

### 8b. Failures → `#errors`

If `state/failures.log` got entries this run, POST a summary to `#errors`. Format:

```bash
curl -s -X POST -H 'Content-Type: application/json' \
  --data @- "$SLACK_WEBHOOK_ERRORS" <<EOF
{
  "blocks": [
    {"type": "header", "text": {"type": "plain_text", "text": "🚨 Jarvis errors — <DATE> <AM|PM>"}},
    {"type": "section", "text": {"type": "mrkdwn", "text": "<N> failures this run:\n• <source>: <one-line error>\n• <source>: <one-line error>"}}
  ]
}
EOF
```

If no failures, skip this POST entirely. Don't spam `#errors` with "no errors today" — silence IS the success signal.

### 8c. Auto-briefings to `#improvements` for ALL 7+/10 items + self-suggestions

**RULE (added 2026-05-19 per Leo):** Every item scored 7/10 or higher in the digest gets an auto-briefing posted to `#improvements`. Plain language. Simple. Not technical.

**Two types of items, two slightly different formats:**

#### Type A — Non-third-party items (Anthropic-native, products you can sign up for, etc.)

Format the briefing as plain text in the commit body, then commit to `briefings/<YYYY-MM-DD>_<item-slug>.md` with this template:

```markdown
# <Item name> — <X>/10

## What it is
<1-2 sentences in plain language. No jargon. Explain like a friend.>

## Why you'd want it (specific to your stack)
<1-2 sentences tied to Leo's projects: writing pipeline, books, Jarvis, fanfic worldbuilding, Council pattern, finals, summer goals. Be SPECIFIC about which project benefits.>

## Why I think it's worth your attention
<1 sentence — the gut take.>

## What to do
<One action: "Apply here", "Enable this flag in settings.json", "Read this post", etc.>

🔗 <URL>
```

Example for Anthropic Dreaming (10/10):
> ## What it is
> A new Anthropic feature that lets Claude agents remember lessons from past sessions, so they get smarter over time without you re-explaining things.
>
> ## Why you'd want it
> Your writing pipeline currently re-builds context every chapter. Dreaming would let it remember craft patterns across chapters automatically — meaning fewer prompt tweaks, better consistency, less work for you.
>
> ## Why I think it's worth your attention
> Harvey saw their agents complete 6× more tasks once they enabled it. The result is conditional on pairing with the Outcomes feature, but the gain is real.
>
> ## What to do
> Apply for the research preview here (gated, takes time).
>
> 🔗 https://www.anthropic.com/news/...

#### Type B — Third-party CODE (Claude skills, MCPs, plugins, agent definitions, etc.)

Per the safety gate (Step 4.5), DON'T install. Briefing still posts to `#improvements` BUT in addition includes the safety-gate deep-dive material. Template:

```markdown
# <Item name> — <X>/10

## What it is
<Plain language, 1-2 sentences.>

## Why you'd want it
<Tied to Leo's projects.>

## Why I think it's worth your attention
<Gut take.>

## What I will do (safety rule)
I won't install this. I'll deep-dive the source, then build a native version for you and test it. Briefing will follow when that's ready.

🔗 <original URL>
```

#### Self-suggestions (config / source / runbook tweaks the agent wants Leo's input on)

Append to `state/agent_suggestions.md` AND write a simple summary file `briefings/<YYYY-MM-DD>_self-suggestions.md` with the same plain-language format (what / why you'd want it / why I want it / what to do).

#### Important: post-ALL-of-them via separate commits

Each 7+/10 item gets its OWN briefing file + its OWN commit (prefix `briefing:`). Multiple commits per run if multiple 7+/10 items exist. Reasoning: each commit fires a separate `#improvements` notification, so Leo can react to each item individually in Slack rather than scrolling a megapost.

If a run has zero 7+/10 items, write no briefings — silence IS the success signal.

### 8c-legacy (direct webhook fallback — keep for emergencies)

This channel fires when commits touch the `briefings/` path. Two types of content land here:

**Type 1 — Deep-dive briefings on 7+/10 third-party items** (per Step 4.5 safety gate):

For each item that passed the safety gate's deep-dive (≥7/10 third-party CODE), write a briefing file to `briefings/<YYYY-MM-DD>_<topic-slug>.md` with this structure:

```markdown
# Briefing: <item name>

**Date:** <YYYY-MM-DD>
**Source URL:** <repo or post URL>
**Score:** <X>/10 (initial: <Y>/10, post-deep-dive adjusted to <X>)
**Category:** Third-party Claude Code skill / MCP / plugin / etc.

## What it does (1 paragraph)

## What it would touch on Leo's system
- Files installed: ...
- Tools requested: ...
- Hooks installed: ...

## Why score >= 7
- ...

## Red flags found during deep-dive
- ... (if any, otherwise "None found")

## Maintainership signal
- Last commit: ...
- Open issues: ... open / ... closed
- Stars / forks: ...

## Recommended action for Leo
- ☐ Approve → Jarvis builds our own version
- ☐ Reject → drop, never surface again
- ☐ Defer → re-evaluate after summer

## Build-our-own sketch (if approved)
<2-3 paragraph sketch of how Jarvis would re-implement the value of this item as Leo-owned code, not third-party install>
```

Then the next-run agent will check `briefings/` for unactioned items, look for Leo's checkbox decision in feedback.md or via a future Slack-reply system.

**Type 2 — General self-suggestions** (config tweaks, source additions, runbook gaps): append to `state/agent_suggestions.md` AND write a brief summary to `briefings/<YYYY-MM-DD>_self-suggestions.md` to fire the `#improvements` channel.

### 8c (legacy direct webhook — keep but de-prioritize)

If for some reason path-filter routing isn't yet set up (Leo hasn't run the `/github subscribe` with path filter), fall back to direct webhook POST per the original Step 8c. Otherwise prefer the file-path approach above — it's reliable, the webhook isn't.

If during the run you noticed something the runbook should change (a dead source, a keyword that produces only noise, a missing source category, a Block Kit formatting bug), append to `state/agent_suggestions.md` AND POST a summary to `#improvements`:

```bash
curl -s -X POST -H 'Content-Type: application/json' \
  --data @- "$SLACK_WEBHOOK_IMPROVEMENTS" <<EOF
{
  "blocks": [
    {"type": "header", "text": {"type": "plain_text", "text": "🛠️ Jarvis improvement suggestions"}},
    {"type": "section", "text": {"type": "mrkdwn", "text": "I noticed during this run:\n\n• <suggestion 1 with reasoning>\n• <suggestion 2 with reasoning>\n\nReact 👍 to approve / 👎 to reject. See <https://github.com/Mr-Fool-00/jarvis-feed/blob/main/state/agent_suggestions.md|state/agent_suggestions.md> for full log."}}
  ]
}
EOF
```

If no new suggestions, skip this POST.

### 8d. Wins → `#wins`

If something in the digest is genuinely worth celebrating (e.g., found a skill that directly addresses Leo's current bottleneck; spotted a meta-pattern that unlocks something), POST to `#wins`. Be sparing — wins channel loses value if every digest gets one.

```bash
curl -s -X POST -H 'Content-Type: application/json' \
  --data @- "$SLACK_WEBHOOK_WINS" <<EOF
{
  "blocks": [
    {"type": "section", "text": {"type": "mrkdwn", "text": "🏆 <one-line description of the win>\n\nFrom <DATE> <AM|PM> digest: <linked title>\n*Why this matters:* <2-3 lines tied to Leo's specific project>"}}
  ]
}
EOF
```

Default: skip this POST. Only fire when there's a real win.

### 8e. Heartbeat → `#general`

At the END of every successful run, POST a brief heartbeat to `#general`:

```bash
curl -s -X POST -H 'Content-Type: application/json' \
  --data @- "$SLACK_WEBHOOK_GENERAL" <<EOF
{
  "blocks": [
    {"type": "context", "elements": [{"type": "mrkdwn", "text": "✓ Jarvis ran <DATE> <AM|PM>. Fetched <N>, surfaced <K>. Wall time: <X> min. <Y> commits. <Z> errors. Full digest in <#ai-news>."}]}
  ]
}
EOF
```

This is the "I'm alive" signal. If `#general` goes silent for 2+ cron cycles, something's broken on the routine side.

### 8f. Gmail backup — DEPRECATED 2026-05-19

**Status: DEPRECATED.** Do not create Gmail drafts anymore.

**Reason:** As of 2026-05-19, the GitHub Actions Slack router handles all per-channel routing reliably. The Gmail-as-fallback step was for the era when direct Slack POSTs from CCR were blocked (which they still are, but the workflow router covers it). Drafts piled up in `grau.enterprises@gmail.com` Drafts that nobody sent — pure waste.

If somehow BOTH the GitHub push AND the Actions workflow fail simultaneously (very unlikely — different infrastructures), log to `state/failures.log` with explicit detail and move on. Leo will see the failures.log change via `#errors` channel on the NEXT successful push.

The Gmail MCP remains attached to the routine for future use cases (e.g. searching past threads for context) but Step 8f no longer fires draft-creation on every run.

### (former 8f content — kept for archival reference only, do NOT execute)

**Sandbox note:** The Gmail MCP exposes `create_draft` but NOT `send_message` (security default). So this step CREATES a draft addressed to leo.p.grau@gmail.com — Leo manually sends it from grau.enterprises@gmail.com's Drafts folder.

To make this less terrible, the draft body should be self-contained — Leo should be able to read it directly without needing the repo or Slack to understand it.

Use the Gmail MCP `create_draft` tool first, then send. Recipient: **`leo.p.grau@gmail.com`** (Leo's personal — the Gmail MCP is bound to `grau.enterprises@gmail.com` (shared with his dad), so the email goes FROM the shared account TO Leo's personal inbox. Do NOT send to `grau.enterprises@gmail.com` — never spam the shared inbox).

**Subject:** `Jarvis digest — <DATE> <AM|PM> — <K> new (top: <truncated top-1 title>)`

**Body (HTML):**
```html
<p><b>Jarvis digest — <DATE> <AM|PM></b></p>
<p>Fetched <N> · New <K> · Surfaced <up to 15></p>

<h3>1. <Title> — <score>/10</h3>
<p><i><source></i></p>
<p><summary></p>
<p><b>Why it matters:</b> <tie></p>
<p>🔗 <a href="<url>"><url></a></p>

<h3>2. <Title> — <score>/10</h3>
...

<h3>3. <Title> — <score>/10</h3>
...

<hr>
<p>Full digest (items 4–15): <a href="https://github.com/Mr-Fool-00/jarvis-feed/blob/main/digests/<filename>">view on GitHub</a></p>
<p><i>To tune: add 👍/👎/🤷 lines to <code>state/feedback.md</code>, commit + push.</i></p>
```

If Gmail MCP fails, log the failure but DON'T abort the run — the digest is already on GitHub.

---

## Step 8.5 — Wave goodbye (cheery closer to #ai-news)

Symmetric with Step 0.5's sendoff. Commit a notify: closer so Leo knows the run finished cleanly. The combination of Step 0.5 + Step 8.5 gives him at-a-glance run health: sendoff fired but no closer in 60+ min = stuck. Both fired = ✓ clean run.

Pick a closer one-liner by day-of-year mod 5 (different rotation than Step 0.5 so they pair distinctly):

```bash
CLOSERS=(
  "🏁 Back from the wilds — digest just dropped above!"
  "✨ Returned with goodies! See digest ↑"
  "📬 Mail call — fresh digest is in. Off to nap until next run."
  "🏞️ Home base, all systems green. Until next run."
  "🎯 Mission accomplished — fresh signal landed above."
)
INDEX=$(($(date -u +%j) % 5))
CLOSER="${CLOSERS[$INDEX]}"

# Optionally include count of items surfaced for at-a-glance signal
if [ -n "${ITEM_COUNT:-}" ]; then
  CLOSER="$CLOSER ($ITEM_COUNT items surfaced)"
fi

# Update heartbeat with completion timestamp
date -u +%Y-%m-%dT%H:%M:%SZ > state/_heartbeat.txt
echo "completed" >> state/_heartbeat.txt

git add state/_heartbeat.txt
git commit -m "notify: $CLOSER"
git push origin main
```

Same failure tolerance as Step 0.5: if push fails, log it and exit cleanly — the digest commit + delivery already happened.

---

## Failure handling

If any **single source** fetch fails (rate limit, parse error, site down):
- Append a line to `state/failures.log`: `<UTC timestamp> <source> <error>`
- Continue with whatever you did fetch
- Include a `## ⚠️ Failures` section in the digest naming the dead sources

If **all sources** fail or you fetch zero items:
- Don't send an empty email
- Still commit a marker file: `digests/<DATE>_<AM|PM>_EMPTY.md` with the failure log
- Email a short failure notice instead (subject: `Jarvis digest — <DATE> <AM|PM> — RUN FAILED`)

---

## Hard rules — never violate

- ❌ NEVER post to social media as Leo
- ❌ NEVER follow / comment on creators (don't actually comment "Skills" on Reels)
- ❌ NEVER make purchases, sign Leo up for things, or auth into accounts
- ❌ NEVER fetch sources that require login — public/anonymous endpoints only
- ❌ NEVER push to branches other than `main`
- ❌ NEVER force-push
- ❌ NEVER delete digest files (they're history)
- ✅ Stay within Anthropic ToS — public web fetches only, respect rate limits
- ✅ **Runtime budget: 55 minutes hard cap. Target 45–50 minutes per run.** Use the time for depth — do not exit early just because the basics are done. Depth IS the value. See "Depth checklist" below.
- ✅ When in doubt about whether something counts as interesting, lean toward **scoring it lower** rather than burning surface space — Leo can always loosen the filter, but a noisy digest erodes trust fast

---

## Depth checklist — use your budget

If you're at the 30-minute mark and basics are done, you have ~20 more minutes to spend. Spend them like this, in order:

1. **Expand source coverage.** Don't stop at the configured fetch limits — paginate deeper on the busy sources (Reddit `after=`, HN `page=`, GitHub `&page=`). Add keyword variations of `hackernews.queries` (e.g. "Claude" → also try "Sonnet 4", "Opus 4", "Anthropic API").
2. **Deep-read top 5.** For the 5 highest-ranked items, fetch the actual content (full Reddit post body via `.json?raw_json=1`, HN comments via Algolia `items/{id}`, GitHub README, RSS full-article fetch). Read it. Base the "why it matters" paragraph on the actual content, not the title.
3. **Multi-pass ranking.** First pass filters obvious noise (one second of judgment per item). Second pass: re-rank the top 30 by deep-reading their summaries. Third pass: cross-reference against `feedback.md` to nudge based on Leo's taste.
4. **Cross-reference linked content.** If a Reddit post links a YouTube video, GitHub repo, or paper, fetch THAT too and bring 1–2 lines of context into the digest item.
5. **Trend section.** If you spot ≥3 items hitting a common theme (e.g. "Claude Skills tooling," "new MCP servers," "agent frameworks shipping this week"), add a `## 📈 Trends spotted` section at the top of the digest naming the theme and the items. This is high-value pattern detection Leo can't get from raw feeds.
6. **Longer "why it matters" on top 3.** Aim for 5–7 lines each, tied to a specific Leo project (writing pipeline, Council skill, Kindle longform, Fate-Anchor worldbuilding, etc.). Be concrete: "This could replace the X step in your Y pipeline" — not "this seems useful."
7. **Self-suggestion log.** As you run, if you spot a source that should be added/removed, or a ranking heuristic that misfires, append to `state/agent_suggestions.md`.

Approaching 55 min? Ship what you have. Never blow past the cap.

---

## Self-improvement note for future runs

If you notice a pattern that should change (e.g. one source is consistently dead, one keyword brings only noise, a new source would be valuable), append a numbered suggestion to a file `state/agent_suggestions.md` (create if needed). Leo reviews this periodically and integrates promising suggestions into `SOURCES.yaml` / `INTEREST_PROFILE.md`. Don't edit those config files yourself — only Leo does that.
