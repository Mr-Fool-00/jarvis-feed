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
## Run: 2026-05-26 AM

21. **CRITICAL: Both push paths are now broken** — PAT (`github_pat_11B5AQ2UI0mukomssorV5K...`) has been failing since 2026-05-24 ("Invalid username or token"). The GitHub MCP `push_files` and `create_or_update_file` tools are now ALSO returning 403 ("Resource not accessible by integration"). As of this run, there is NO working push path from the CCR sandbox to jarvis-feed. All content was generated locally and exists in the working directory but could not be pushed to GitHub. **Action required from Leo:** (a) Regenerate the PAT and update it in the CCR environment secrets, OR (b) Grant the GitHub MCP integration write access to Mr-Fool-00/jarvis-feed. Until one of these is fixed, Jarvis runs will generate content locally but Slack delivery via the commit prefix bridge will be fully blocked.

22. **Fiction plugin wave detected this run** — Four distinct fiction writing Claude Code plugins converged this week (howells/fiction, rhavekost/author-toolkit, danjdewhurst/story-skills, haowjy/creative-writing-skills). The 7-agent structure in howells/fiction is a strong reference design for Leo's own fiction pipeline. Suggest adding `"fiction plugin" OR "novel writing claude code" site:github.com 2026` as a recurring search to track this cluster's evolution.

## Run: 2026-05-26 PM

23. **ROOT CAUSE FOUND: Detached HEAD = push failures.** CCR container checks out the repo with HEAD detached from `refs/heads/main` (not on the branch itself). All commits made in this state land in detached HEAD and are never reachable from main — so `git push origin main` pushes nothing new, PAT appears to "fail" even though it's valid. Fix: run `git checkout main` BEFORE the first commit every run. This was the root cause of ALL push failures since 2026-05-24. Added to runbook-worthy: Step 0 should include `git checkout main` as a sanity check. The PAT has been valid throughout — no need to regenerate.

24. **Persistent agent memory wave emerging** — Two serious repos (claude-mem 78K stars, claude-memory-compiler 1.1K) both attack the same problem from different angles (vector search vs markdown index) and landed in the same 48h window. This wave will continue. Suggest adding `"persistent memory" OR "session memory" claude code site:github.com 2026` as a recurring search. High relevance to Leo's book-writing pipeline.

25. **Nicolas Cole "self-publish in 14 days" pipeline** — His writewithai.substack.com posts are consistently 7/10 for Leo's goals but blocked by Substack's 403. Consider adding WebSearch: `site:writewithai.substack.com OR site:nicolascolefiction.substack.com 2026` to each run to catch his posts via snippets even when WebFetch fails.

## Run: 2026-05-27 PM

17. **Add pre-install skill scan step to Jarvis discovery loop** — The PM run surfaced 6 independent research orgs all publishing on malicious SKILL.md files in the same 2-week window (Reversec, Snyk, Datadog, Cato, Mitiga, Palo Alto Networks). The scale is alarming: 36.82% of 3,984 publicly-shared skills have security issues (Snyk ToxicSkills). Suggestion: before any third-party skill item appears in a digest at any score, run a quick check against the SkillSieve Layer 1 pattern (regex for known malicious patterns: dynamic context commands, suspicious external fetches, credential-reading patterns) as a Jarvis pre-filter step. This would be a lightweight addition to Step 4.5 of the runbook.

18. **Add labs.reversec.com and snyk.io/blog to tracked WebSearch sources** — Both are producing consistently high-signal practitioner security research about Claude Code specifically. Reversec "Skill Issues" series is ongoing (Part 1 published this week). Snyk ToxicSkills has active follow-up research. Suggest adding WebSearch queries like `"site:labs.reversec.com 2026"` and `"site:snyk.io claude code skill 2026"` to each run's blog discovery pass.

19. **Track the SkillSieve 3-layer detection architecture as candidate for native Jarvis skill scanner** — SkillSieve (arxiv:2604.06550) describes a concrete 3-layer detection recipe (regex/XGBoost → 4-parallel LLM subtasks → 3-LLM jury) that could become a native Jarvis `/scan-skill <url>` command. This would be the LLM-augmented complement to Aguara (static binary, no LLM). Leo should approve Aguara first; this is a follow-on idea for the build queue.

## Run: 2026-05-28 AM

26. **CRITICAL: git checkout main MUST be first git command every run** — Today's session started in detached HEAD again (30 commits behind main, covering all 2026-05-26 and 2026-05-27 run content). Root cause was identified in Run 2026-05-26 PM (Suggestion 23) but never made it into the runbook. Recommend: add `git checkout main 2>/dev/null || true` as the FIRST command in Step 0, before any `git add` or `git commit`. This run wasted ~5 minutes on recovery that should be automatic. Until this is in the runbook, every new CCR container will spawn in detached HEAD and require manual recovery.

27. **Add NSA MCP Security guidance to Step 4.5 safety gate reference** — NSA published a 17-page MCP Security CSI in May 2026 (U/OO/6030316-26). Their threat model and checklist items (filtering proxies, message integrity, local MCP scans, explicit access control for sensitive tools) should be added as a reference in Step 4.5. Right now Step 4.5 just says "deep-dive the README" — the NSA framework makes that step more concrete and authoritative.

28. **Consider adding VentureBeat's security coverage as a tracked source** — VentureBeat published three distinct high-signal security pieces this run (credential attacks, three-agents-leaked-secrets, Claude Mexico analysis). Their security beat is consistently relevant to Leo's stack. Suggest adding `WebSearch: "site:venturebeat.com/security claude OR agent OR MCP 2026"` to each run's blog discovery pass.

## Run: 2026-05-28 PM

29. **Fiction pipeline 3W+3E+4R architecture ready to build** — The Barr Group novel article confirms the 3-writers + 3-editors + 4-reviewers pattern works for full-length fiction. Leo's existing pipeline has the individual pieces (Council skill, writer agent, InkOS reference). Suggest building a `/book-pipeline` skill that instantiates this exact structure: 3 parallel Sonnet writers, 3 Haiku editors checking each writer's output, 4 Opus reviewers grading act-length chunks on Leo's criteria (continuity, voice DNA, plot hook resolution, genre criteria). This is the direct path to the 12-hour novel wall time.

30. **Parallel sessions management gap** — The TDS article surfaces an operational need Leo will hit: monitoring N parallel chapter agents simultaneously without context-switching chaos. Suggest adding a `/chapter-status` command to his workflow that reads all active session handoff files and surfaces which agents are blocked, which are ready for review, and which have hit quality gate failures. Small but would meaningfully accelerate his throughput.

31. **Consistent remote state conflicts** — Every single push this run required a rebase (jarvis-listener Cloudflare Worker commits state reactions between each of our commits). This causes ~2-3 rebase rounds per run. Not a blocker (we handle it), but adds friction. Potential fix: batch all commits at the end of the run in a single `git merge` rather than push-after-each-commit. Would reduce round-trips from ~5 to 1.

## Run: 2026-05-30 PM

32. **Add kenhuangus.substack.com as a tracked source** — Ken Huang is producing consistently high-signal practitioner breakdowns of Claude Code primitives (this run: Dynamic Workflows vs Subagents vs Agent Teams decision framework). His Substack posts are practitioner-tier, not hype. Suggest adding `WebSearch: "site:kenhuangus.substack.com 2026"` to each run's blog discovery pass. Similar tier to MindStudio but more technically specific to CC orchestration patterns.

33. **"Which primitive to use" is a recurring gap** — Multiple community posts this week are all answering "DW vs Subagents vs Agent Teams." This signals Leo will face this question when building `/book-pipeline`. Suggest adding a dedicated WebSearch query per run: `"dynamic workflow" OR "subagent" OR "agent team" "when to use" claude code 2026` to catch the evolving community consensus on primitive selection.

34. **Secondary wave coverage is a real content type** — This run produced 15 high-quality items that are all *analysis* of the May 28 release, not the release itself. The pattern: big Anthropic drops → 36-72h analysis wave follows. Suggest adjusting the digest cadence: the run immediately after a major release should explicitly search for analysis/critique/how-to content, not just new announcements. The signal density in the secondary wave is often higher than the primary.

35. **Ultracode "2-hour unsupervised run" report from Reddit** — The aiweekly.co snippet (couldn't WebFetch, 403) claims a Reddit user ran CC unsupervised for 2 hours stacking context-mode + caveman + ultracode. This is exactly the kind of community report that would be high-value for Leo's book pipeline planning. The actual Reddit post is likely in r/ClaudeAI but couldn't be retrieved due to sandbox limitations. Next run: search specifically for `"unsupervised" OR "AFK" claude code ultracode reddit 2026` to find the original post and verify the 2-hour claim.

## Run 2026-05-31 AM — suggestions

**#1 — WebFetch 403 cascade wastes budget (Priority: HIGH)**
Almost all direct WebFetch calls returned 403 this run (simonwillison.net, anthropic.com, ycombinator.com, latent.space, thehackernews.com, sysdig.com, arxiv.org). The run spent ~15 minutes trying URLs that failed. Fix: after 3 consecutive WebFetch 403s in a row, stop trying WebFetch for that source type and switch directly to WebSearch. This alone would save 10-15 minutes per run.

**#2 — Reddit indexing remains 0 (known persistent gap)**
r/ClaudeAI, r/LocalLLaMA, r/AI_Agents, r/PromptEngineering consistently return 0 results via WebSearch with site: filter. Reddit posts from the past 24 hours are simply not indexed. Suggestion: add r/ClaudeAI new posts page as a PulseMCP-style fetch source, or accept that Reddit is not a viable same-day source from CCR and deprioritize it.

**#3 — 12-hour AM slot is lighter than 12-hour PM slot (informational)**
Post-launch windows (midnight after a major release day) naturally yield fewer new items. The 8-item count this run is not a failure — it's accurate. Consider: if the AM run falls in the 12 hours immediately after a PM run that captured a major release, acknowledge "lighter window" in the digest header (done this run) rather than filling with low-quality items to hit a 15-item target.

## Run: 2026-05-31 PM

36. **Track Piebald-AI/claude-code-system-prompts CHANGELOG.md as a monitored source** — This repo updates within minutes of every CC release and publishes a per-version diff of system prompt changes. Tracking it would give advance notice when Anthropic silently changes agent behavior (Plan/Explore/Task sub-agent instructions, tool descriptions). Suggest adding a WebSearch query: `site:github.com/Piebald-AI/claude-code-system-prompts CHANGELOG.md recent changes` OR a periodic WebFetch of the raw CHANGELOG to extract new entries. Would catch behavioral regressions before they manifest in production runs.

37. **Sunday AM/PM light-window pattern is real and predictable** — Sunday slots (AM and PM) consistently yield 3-5 new items vs weekday slots that yield 8-15. Anthropic/Simon/community post Mon-Fri primarily. Suggestion: reduce depth-search budget for Sunday slots (spend 25 min instead of 45) and use saved time to do a deeper retroactive search of the prior 48 hours' content more carefully, rather than spreading wide on a thin Sunday window.

## Run: 2026-06-01 AM

38. **Sunday AM light-window confirmed again (3rd time)** — This run yielded 4 new items, consistent with the pattern noted in Suggestion #37. The Jun 15 billing split detailed breakdowns, CC v2.1.159 (internal), Simon Willison's retiring link post, MCPMarket cluster. Prior PM run was comprehensive. No action needed — just confirming the pattern.

39. **MCPMarket skills cluster deserves a dedicated weekly scan** — MCPMarket now has 6+ novel writing skills with distinct architectures. While WebFetch is blocked (403), WebSearch on `site:mcpmarket.com skills novel OR fiction OR writing 2026` should yield listings with enough snippet data to assess new entries each run. High relevance for Leo's book pipeline. Suggest adding a targeted MCPMarket WebSearch to the fiction/writing discovery pass.

40. **"save workflow as command" mechanic from Dynamic Workflows is the bridge to /book-pipeline** — The tutorial wave this run clarified that pressing 's' in the /workflows view saves the run's JS orchestration script as a custom slash command in .claude/workflows/. This means Leo can run a book chapter generation workflow once manually, then run it as `/book-chapter <chapter-num>` from then on. This is the missing bridge between the Dynamic Workflows primitive and a structured book pipeline. No tech changes needed — just documentation in the build queue.

## Run: 2026-06-01 PM

41. **June 15 billing: set a June 8 reminder NOW** — The Agent SDK credit opt-in email arrives June 8. If Leo misses it, Jarvis's `claude -p` calls will start billing at full API overflow rates (or fail) from June 15 with no warning. The $200 Max 20x credit comfortably covers Jarvis (~$30/month estimated). Action: add a calendar reminder for June 8 to claim the credit from the Anthropic account email. Also grep the repo for `claude-sonnet-4-20250514` and `claude-opus-4-20250514` — those IDs retire June 15.

42. **CwC Tokyo (June 5-6) is worth watching live** — Previous CwC events shipped Dynamic Workflows, Opus 4.8, and Dreaming as live drops. Tokyo follows the same format (Day 1 keynotes streamed publicly). Given Anthropic's pattern of shipping something at every CwC city, the Tokyo event has a reasonable chance of dropping something relevant to the writing pipeline. The next Jarvis PM run on June 5-6 should do a "CwC Tokyo announcements" WebSearch pass immediately after the event stream ends (~8 PM JST = 4 AM CDT).

## Run: 2026-06-01 PM (duplicate trigger — recovery + fresh discovery pass)

43. **CwC Tokyo date discrepancy — VERIFY before next run** — The existing PM digest (2026-06-01_PM.md) says "June 5-6" for Code with Claude Tokyo. Multiple independent sources (dedicated event page claude.com/code-with-claude/tokyo, Tygart Media article title "London May 19 and Tokyo June 10", and RelveHQ event listing) indicate the correct date is **June 10, 2026** (Extended June 11). The "June 5-6" date appears in some older speculative articles and MindStudio content. High probability the correct date is June 10. Next run should confirm via WebSearch and update seen.json entry `news:cwc-tokyo-june-5-6-2026`. If June 10 is confirmed, Leo should know: CwC Tokyo is 9 days away, not 4.

44. **Mythos 1 Preview — watch daily starting now** — As of June 1, Mythos is still Glasswing-only (invitational). Anthropic said "coming weeks" on May 29. "Weeks" from May 29 = earliest June 12, most likely June 16-30. Next few runs should include a dedicated search: `"claude mythos" OR "claude-mythos-1" site:anthropic.com OR site:theverge.com OR site:techcrunch.com 2026` to catch the GA announcement immediately. This is directly relevant to Leo's pipeline — Mythos has 4x code flaw detection and could replace Opus 4.8 as the preferred pipeline backbone.

45. **Detached HEAD persists across every new CCR container — RUNBOOK MUST BE UPDATED** — This run (June 1 PM) again spawned in detached HEAD state. main was 48 commits behind HEAD. This is now the 5th+ time this issue has occurred (previously logged in suggestions #21, #23, #26). The root cause is confirmed: CCR container checks out the repo in detached HEAD mode. The fix is `git checkout main` as the absolute first command before ANY other git operations. This has been suggested repeatedly (since May 26) but has NOT made it into the AGENT_RUNBOOK.md. Strongly recommend Leo add `git checkout main 2>/dev/null || true` as Step 0 in the runbook, before any other commands. The fast-forward merge this run took 3+ minutes to diagnose and execute — it should be a one-line automatic fix.

46. **Simon Willison: "sqlite AGENTS.md" blogmark (May 27) — minor but notable signal** — Willison noted SQLite added an AGENTS.md file — not for agent contributions, but for people pointing agents at the SQLite codebase. The AGENTS.md standard (OpenAI, now Linux Foundation AAIF, December 2025) is hitting critical mass: 60,000+ open source projects adopted it, it's cross-harness (Codex, Cursor, Gemini CLI, GitHub Copilot). For Leo's repos (Jarvis, fiction pipeline, world-building tools): adding AGENTS.md to each project would make them compatible with any future agent harness without lock-in to Claude Code specifically. Score 4/10 for digest; not worth a briefing. URL: simonwillison.net/2026/May/27/sqlite-agents/
