# Agent self-improvement suggestions

Format: `<date> [suggestion] <reasoning>`

---

## Run: 2026-05-19 PM

1. **Add Lenny's Newsletter RSS to SOURCES.yaml** — `https://www.lennysnewsletter.com/feed`. Caught the Boris Cherny piece via WebSearch but almost missed it. Lenny consistently publishes high-signal practitioner interviews (Boris Cherny, Adam D'Angelo, etc.) — the Claude Code / AI builder tier specifically. Should be a first-class source.

2. **Add arxiv cs.AI/cs.CL WebSearch as a source** — Currently no papers coverage. This run surfaced a legitimate paper (ORCH multi-agent orchestrator, 2602.01797) via accident. A weekly scan for "agent" + "prompt" + "tool use" + recent date would catch practical papers Leo can apply.

3. **Reddit direct JSON API blocked — consider adding Reddit via alternative** — The site:reddit.com WebSearch approach returned almost nothing. Reddit is a configured primary source in SOURCES.yaml but produced 0 items this run due to sandbox curl block + WebSearch indexing lag. Options: (a) switch Reddit to WebSearch with broader queries ("r/ClaudeAI top posts", "LocalLLaMA Claude discussion this week"), (b) add Lemmy or alternative communities as backup, (c) accept Reddit coverage is sparse and compensate with HN/GitHub depth.

4. **Add Pragmatic Engineer newsletter to RSS sources** — Gergely Orosz published a piece "Building Claude Code with Boris Cherny" that would score 9/10 for Leo but wasn't in any configured source. URL: `https://newsletter.pragmaticengineer.com/feed`. Very high signal-to-noise, practitioner-tier.

5. **Add PulseMCP.com to sources** — Surfaced as a MCP server registry during this run. Acts as a curated index of new MCP servers (like ProductHunt but for MCP). WebFetch or WebSearch against pulsemcp.com/servers/new weekly would catch Leo-relevant MCP releases faster than GitHub search.

6. **HN deep-read blocked** — Direct WebFetch on news.ycombinator.com/item?id=* returns 403 in this sandbox. As a workaround, WebSearch for the headline + "hacker news" extracts snippet context. Not ideal — we miss comment threads which sometimes contain the best insight (e.g., "this architecture is similar to X which failed because Y"). If sandbox allows fetching via Algolia, try `https://hn.algolia.com/api/v1/items/{id}` as a WebFetch fallback before curl.

## Run: 2026-05-20 PM

7. **Add Claude Code changelog page as a tracked source** — The changelog at code.claude.com/docs/en/changelog is updated multiple times per week (v2.1.144 yesterday, v2.1.145 today). Currently we catch it via WebSearch which is laggy. A direct WebFetch on the changelog page at run-start would catch same-day releases reliably. Add to SOURCES.yaml as an rss/webfetch source.

8. **Track the Advisor Tool ecosystem** — The advisor-tool-2026-03-01 beta is generating a cluster of blog posts (builder.io, MindStudio, chatgptguide.ai) that contain useful benchmark data and implementation patterns. These practitioner analyses are higher-signal than the official docs for Leo's use case. Consider adding a WebSearch query "claude advisor tool" to the HN/blog discovery pass.

9. **YouTube London keynote missed** — The Code w/ Claude London Day 1 keynote (youtube.com/watch?v=6amLO7I9xdg) was not fetchable this run (403). Should be fully indexed within 24 hours. Next run (2026-05-20 AM cycle or 2026-05-21 AM): retry WebFetch on that video URL or search for transcript.
