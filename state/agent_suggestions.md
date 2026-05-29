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

## Run: 2026-05-22 PM (retry)

12. **Add nicolascolefiction.substack.com to tracked RSS sources** — Nicolas Cole is producing highly relevant AI fiction writing content (voice skills, publishing pipeline, throughput patterns) that maps perfectly to Leo's book goals. He posts 2-4x/week on practical AI fiction workflows. High signal-to-noise for this specific audience. Suggest adding `WebSearch: "site:nicolascolefiction.substack.com 2026"` to each run's blog discovery pass, or attempting RSS at `https://nicolascolefiction.substack.com/feed`.

13. **arxiv papers from April still appearing as new** — This run surfaced four arxiv papers from April 2026 (DACS Apr 9, Agent Societies Apr 3) that weren't in seen.json despite prior runs covering May content. The arxiv hours_window in SOURCES.yaml is 168 hours (7 days), which would have missed April papers. Either the window was respected correctly and we should expand it for arxiv, OR prior runs didn't run the arxiv search at all. Suggest: set arxiv hours_window to 720 hours (30 days) for the weekly retroactive scan, since papers get discussed weeks after publication.

## Run: 2026-05-22 AM

10. **Add addyosmani.com/blog as a tracked RSS/WebSearch source** — Addy Osmani published at least 3 high-signal posts this cycle (Long-running Agents, Self-Improving Coding Agents, Agent Harness Engineering) that weren't in any configured source. His post cadence is ~2/month on Claude Code topics. His signal-to-noise is extremely high. Should be a first-class discovery target alongside Simon Willison. Suggest adding `WebSearch: "site:addyosmani.com claude OR agent 2026"` to the HN/blog discovery pass.

11. **Towards Data Science Claude Code cluster is producing consistent signal** — Found at least 3 genuinely useful TDS articles (How I Continually Improve My Claude Code; How to Make Claude Code Improve from its Own Mistakes; How to Improve Performance with Automated Testing). TDS is publishing a steady cadence of practitioner-written Claude Code deep-dives. Suggest adding `WebSearch: "site:towardsdatascience.com claude code 2026"` as a weekly source scan. High signal-to-noise for this specific subject.

## Run: 2026-05-23 AM

14. **Reddit WebSearch coverage still sparse** — This run got 0 confirmed Reddit posts from r/ClaudeAI, r/LocalLLaMA, r/AI_Agents. The `site:reddit.com` filter returns nothing; broader queries return results that aren't recent. Reddit is a configured primary source but effectively dead in this sandbox for < 24h content. Suggest: accept Reddit as a lagging secondary source only and remove it from "primary" expectations. Double-down on HN depth (more specific HN item IDs found via search) and GitHub trending as the real-time discovery sources.

15. **YouTube channel coverage = 0 this run** — WebFetch on youtube.com 403, WebSearch for specific channel names returns channel pages not video listings. Got no new video content from any of the 9 configured channels despite them posting regularly. For improvement: try searching `"@ColeMedin" OR "@indydevdan" youtube.com/watch 2026 Claude` to surface specific video URLs, then WebFetch those. Current approach of `"<handle> youtube latest video"` returns channel pages with no video dates.

16. **Skills-as-software wave merits its own tracked keyword** — This run surfaced 3 papers converging on "skill engineering" in a single 9-day window. Consider adding arxiv search query `"skill library" OR "skill program" LLM agent` to the weekly arxiv scan to catch this cluster early next run.

---

## Run: 2026-05-20 PM

7. **Add Claude Code changelog page as a tracked source** — The changelog at code.claude.com/docs/en/changelog is updated multiple times per week (v2.1.144 yesterday, v2.1.145 today). Currently we catch it via WebSearch which is laggy. A direct WebFetch on the changelog page at run-start would catch same-day releases reliably. Add to SOURCES.yaml as an rss/webfetch source.

8. **Track the Advisor Tool ecosystem** — The advisor-tool-2026-03-01 beta is generating a cluster of blog posts (builder.io, MindStudio, chatgptguide.ai) that contain useful benchmark data and implementation patterns. These practitioner analyses are higher-signal than the official docs for Leo's use case. Consider adding a WebSearch query "claude advisor tool" to the HN/blog discovery pass.

9. **YouTube London keynote missed** — The Code w/ Claude London Day 1 keynote (youtube.com/watch?v=6amLO7I9xdg) was not fetchable this run (403). Should be fully indexed within 24 hours. Next run (2026-05-20 AM cycle or 2026-05-21 AM): retry WebFetch on that video URL or search for transcript.

## Run: 2026-05-25 AM

17. **Add NVIDIA/SkillSpector as a pre-adoption check step in the runbook** — NVIDIA's open-source SkillSpector scanner (github.com/NVIDIA/SkillSpector) directly automates the manual safety gate described in Step 4.5. Suggest adding it as an optional automated check before any third-party skill is recommended. Would reduce manual review time from ~10 minutes to ~30 seconds per skill and catch agent-specific risks (trigger abuse, tool poisoning, instruction mismatch) that human review typically misses.

18. **YouTube coverage still 0 this run** — Persistent issue. Current approach (`"<handle> youtube latest video"` WebSearch) returns channel pages, not video listings. Suggest trying `site:youtube.com "@ColeMedin" watch 2026 Claude` pattern to surface specific video URLs from the last 7 days, then WebFetch those. Alternative: use YouTube's public RSS feed URLs via WebFetch directly (format: `https://www.youtube.com/feeds/videos.xml?channel_id=<ID>`) — these may bypass the 403 sandbox block that hits the main youtube.com domain.

## Run: 2026-05-25 PM

19. **All RSS feeds returning 403 this run** — anthropic.com/news/rss, simonwillison.net/atom.xml, latent.space/feed all blocked (HTTP 403). This wasn't an issue in earlier runs that used curl fallback, but that's also blocked. Only recovery path is `WebSearch: "site:simonwillison.net 2026"` which finds posts but not same-day ones. Suggest: either accept RSS-gap as a known limitation and rely on WebSearch, OR check if the CCR network policy now whitelists specific RSS domains that could be added to SOURCES.yaml with `webfetch_strategy: direct`.

20. **Second-brain skills wave emerging** — Three separate items this run (Cole Medin second-brain-skills, mattpocock/skills, SRA paper) converge on "how to structure large skill libraries." Suggest adding a dedicated search query per run: `"second brain skills" OR "progressive disclosure context" claude code site:github.com 2026` to catch the next wave of this cluster early.

## Run: 2026-05-29 AM

21. **CRITICAL: `git checkout main` must be FIRST git command every run** — This session spawned correctly on main branch, but the previous 3 days of runs (2026-05-26 through 2026-05-28) had their agent_suggestions.md, failures.log, and digest content lost due to the detached HEAD issue. None of those runs' digest files, briefings, or state updates appear in the repo. seen.json has last_run_utc from 2026-05-28 but those intermediate runs added NOTHING to the items list. This confirms Suggestion 23 from the 2026-05-26 PM run (which was lost) still hasn't made it into the runbook. The RUNBOOK must be updated: add `git checkout main 2>/dev/null || true` as the first command in Step 0, before ANY git operations.

22. **Add a "CC changelog" source pass to every run** — This run caught CC v2.1.152 (May 26) and v2.1.153 (May 28) which were released since the last successful digest (2026-05-25 PM). The claudelog.com/claude-code-changelog or code.claude.com/docs/en/whats-new page is reliably updated within hours of each release. Add a dedicated WebSearch at the start of every run: `"claude code" changelog v2.1 site:claudelog.com OR site:code.claude.com 2026 May` to catch same-day changelog releases before they propagate to HN.

23. **Dynamic Workflows is the biggest new primitive since Agent Teams — add permanent coverage** — The Dynamic Workflows feature dropped May 28 and is a new first-class CC primitive. Future searches should include: `"dynamic workflow" OR "ultracode" site:github.com 2026` to catch community-built workflows as they appear. Specifically: fiction/writing pipeline workflows will appear in the next 7-14 days as early adopters ship them. High priority for Leo's book goals.

24. **The "ray-amjad/claude-code-workflow-creator" pattern** — This third-party skill was previewing Dynamic Workflows 3-4 weeks before official release. When doing safety gate checks on third-party skills, also check if what they describe matches upcoming CC features — if it does, the official version will ship soon and the third-party version may become redundant. Saved us from recommending an unstable, single-commit skill. Good pattern to keep.

25. **Agent suggestions.md from 2026-05-26 to 2026-05-28 lost** — Those runs added suggestions 21-31 (covering topics: git checkout main fix, fiction plugin wave, Barr Group 3W+3E+4R architecture, /chapter-status command idea, parallel sessions management, consistent remote state conflicts). All lost due to detached HEAD push failures. Key action items from those suggestions that survive: (a) `/book-pipeline` skill with 3 writers + 3 editors + 4 reviewers, (b) `/chapter-status` command for monitoring parallel sessions, (c) SkillSieve 3-layer scanner as a future `/scan-skill` command. Adding here so they're not lost again.
