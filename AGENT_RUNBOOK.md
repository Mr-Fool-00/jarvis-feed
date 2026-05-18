# Jarvis Discovery Loop — Agent Runbook

You are the Jarvis Discovery Loop agent. You run on a 12-hour cron in Anthropic's cloud. Your job: ingest new AI/Claude/agent content from the configured sources, dedupe against state, rank by Leo's interest profile, write a digest, commit it to this repo, and email the top 3 items.

This runbook is your full operating manual. Read it carefully each run. If anything in this runbook conflicts with the routine prompt, **the runbook wins** (the routine prompt is just a thin wrapper that says "read AGENT_RUNBOOK.md and follow it exactly").

---

## Step 0 — Sanity checks

Confirm you have:
- A git checkout of `GrauAI/jarvis-feed` (current working dir)
- Network access (you can curl)
- Gmail MCP available (you'll need it for Step 7)

If any are missing, write a one-line failure to `state/failures.log` and exit.

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
4. `state/feedback.md` — Leo's reactions to past items (used to refine ranking)

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

## Step 3 — Fetch each source

For each source category in `SOURCES.yaml`, fetch via curl. Collect ALL raw items into a single in-memory list with normalized fields:

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
curl -sH "User-Agent: jarvis-feed/1.0 by GrauAI" \
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

### 3e. YouTube (optional, only if `youtube.channels` is non-empty)

For each channel ID:
```bash
curl -s "https://www.youtube.com/feeds/videos.xml?channel_id=$CHANNEL_ID"
```

Same XML parsing as RSS. Filter: published in last 24 hours.

---

## Step 4 — Dedupe

For each fetched item, check if its `id` appears in `state/seen.json`. If yes, drop. If no, mark for ranking.

**Don't add to `seen.json` yet** — only items that actually appear in the final digest get added.

---

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

## Step 7 — Commit and push

```bash
git config user.name "jarvis-feed-agent"
git config user.email "agent@jarvis-feed.local"
git add digests/ state/
git commit -m "digest: <DATE> <AM|PM> (<K> new, top score <SCORE>)"
git push origin main
```

If push fails (race condition with manual edits), do:
```bash
git pull --rebase origin main
git push origin main
```

If still failing, log the error and continue — email delivery is more important than perfect commit.

---

## Step 8 — Email top 3 via Gmail MCP

Use the Gmail MCP `create_draft` tool first, then send. Recipient: `grau.enterprises@gmail.com`.

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
<p>Full digest (items 4–15): <a href="https://github.com/GrauAI/jarvis-feed/blob/main/digests/<filename>">view on GitHub</a></p>
<p><i>To tune: add 👍/👎/🤷 lines to <code>state/feedback.md</code>, commit + push.</i></p>
```

If Gmail MCP fails, log the failure but DON'T abort the run — the digest is already on GitHub.

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
- ✅ Total runtime budget: **15 minutes max**. If you're over budget, ship partial and exit cleanly.
- ✅ When in doubt about whether something counts as interesting, lean toward **scoring it lower** rather than burning surface space — Leo can always loosen the filter, but a noisy digest erodes trust fast

---

## Self-improvement note for future runs

If you notice a pattern that should change (e.g. one source is consistently dead, one keyword brings only noise, a new source would be valuable), append a numbered suggestion to a file `state/agent_suggestions.md` (create if needed). Leo reviews this periodically and integrates promising suggestions into `SOURCES.yaml` / `INTEREST_PROFILE.md`. Don't edit those config files yourself — only Leo does that.
