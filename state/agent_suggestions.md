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

## Run: 2026-06-02 AM

47. **CwC Tokyo date confirmed — June 10 main, June 11 Extended (resolves #42 and #43)** — WebSearch confirmed via official claude.com/code-with-claude/tokyo page. The PM digest date of "June 5-6" was incorrect. Correct dates: June 10 (main keynotes + demos), June 11 (Extended, indie devs + founders). This is 9 days away from this run. Suggestion #42's "PM run on June 5-6 should do a CwC search" is now moot — updated to: PM run on June 10 should do a "CwC Tokyo announcements" WebSearch pass immediately when the event ends (~8 PM JST = 6 AM CDT). No SOURCES.yaml change needed.

48. **Add NousResearch/autonovel adversarial review pattern to fiction pipeline build queue** — NousResearch shipped a working 79K-word autonomous novel pipeline with dual-Opus-persona review (literary critic + fiction professor), chapter-level quality gate (retry if score < 6.0), and embedded craft constraint documents (ANTI-SLOP.md, ANTI-PATTERNS.md, CRAFT.md). This is the highest-fidelity public reference for Leo's `/book-pipeline` skill. Specifically: the ANTI-SLOP.md banned word list (21 words) can be pasted directly into fiction CLAUDE.md today with zero code changes. The dual-Opus-review pattern maps directly to the Council skill pattern Leo already uses. Suggest building the adversarial review step as the next addition to the fiction pipeline, informed by this architecture.

## Run: 2026-06-03 AM

49. **MCP tool overload degrades accuracy — add "MCP tool count per session" as a pipeline design rule** — Hermes Tool Search data (May 29) shows Claude Opus 4 accuracy drops from 74% to 49% when exposed to 34+ MCP tools simultaneously. 50% of context per turn consumed by tool schemas. Anthropic's own evals confirmed this. Suggest adding a design rule to Leo's CLAUDE.md and any fiction pipeline documentation: cap concurrent MCP tools at ≤20 per session, scoped to the active task. This is cheap to implement now (before the pipeline has 30+ tools) and prevents a silent quality regression as the setup scales.

50. **Single-agent-with-skills architecture likely produces better prose per token than parallel writers** — The equal-token-budget research (arxiv:2604.02460 + 2601.04748) challenges the 3W+3E+4R parallel writer architecture: multi-agent coordination costs 4-15x tokens for equivalent quality. For Leo's fiction pipeline, this suggests: use a single high-quality Sonnet agent per chapter (with the right fiction skills loaded), not 3 parallel writers who each get 1/3 of the token budget. Run writers in parallel for throughput, but each writer should be solo within its chapter's context window. The reviewers/editors can be lighter — their job is gate-keeping, not co-creation. Suggest testing single-writer vs 3-parallel-writer on a sample chapter before committing to the architecture.

51. **CwC Tokyo is June 10 — AM run on June 10 should lead with a Tokyo announcements pass** — The June 10 keynote (8 PM JST = 6 AM CDT) ends right around when the next AM cron would fire. The June 10 AM digest should have a dedicated "CwC Tokyo announcements" WebSearch pass as the FIRST discovery step, ahead of all normal source coverage. Expected content: new Claude Code features, new model hints, possibly Mythos 1 release preview, long-horizon task infrastructure announcements. High-probability hit for 8+/10 items.

## Run: 2026-06-03 PM

52. **Add addyosmani.com/blog as a tracked RSS/WebSearch source** — This PM run surfaced a high-signal April 2026 post (Agentic Engine Optimization framework) that wasn't in seen.json despite being 50+ days old. Osmani's blog publishes 2-3 high-signal Claude Code / agent architecture posts per month. The AEO post alone is directly applicable to Leo's SKILL.md documentation quality. Suggest adding `WebSearch: "site:addyosmani.com claude OR agent 2026"` to the HN/blog discovery pass (Suggestion 10 from May 22 AM also called for this but it was never actioned in SOURCES.yaml). This run confirms the signal is consistently high.

53. **Dynamic Workflows analysis wave merits a dedicated search pass for the next 2 weeks** — Five+ practitioner analysis posts appeared in 6 days after Dynamic Workflows launched May 28. The wave will continue as more builders experiment. Suggest adding a targeted search query for the next 4 runs: `"dynamic workflow" site:mindstudio.ai OR site:addyosmani.com OR site:infoq.com 2026` to catch practical pattern guides as they publish. High relevance to Leo's fiction pipeline architecture. Remove this search after June 17 (2 weeks post-launch).

54. **Glasswing/Mythos 1 general availability signal is accelerating — add monitoring** — Anthropic has now expanded Glasswing from ~50 to ~200 organizations in ~6 weeks. The "coming weeks" language for Mythos 1 preview was used on May 28 and is now 6 days old. Suggest adding `"claude mythos 1 preview" OR "mythos general availability" site:anthropic.com 2026` as a weekly search. A Mythos 1 release would likely be a 10/10 digest item and warrants catching on day-of.

## Run: 2026-06-04 AM

55. **Cross-reference billing/policy items against official Anthropic support docs before scoring** — The June 3 AM run surfaced `billing:june15-interactive-terminal-workaround` (scored 4) with the claim "Routines (ACP path) DO hit credit pool." This turned out to be wrong per official Anthropic support documentation: Claude Cowork (including CCR routines) is explicitly exempt. A higher-scored wrong item is worse than a lower-scored correct one. Suggest: when surfacing billing/policy items that could affect Leo's pipeline directly, add a mandatory cross-reference check against `support.claude.com/en/articles/*` before finalizing the score. If the official doc contradicts the search snippet, the official doc wins and the item gets flagged as "corrected" rather than surfaced at face value.

## Run: 2026-06-05 AM

56. **Add security research sources (GMO Flatt Security, SecurityWeek Claude beat) to SOURCES.yaml blog discovery pass** — Both produced 7/10 items this run (RyotaK GitHub Actions flaw; "Comment and Control" prompt injection). Neither is in SOURCES.yaml. Suggest adding: `WebSearch: "site:flatt.tech/research claude OR claude-code 2026"` and `WebSearch: "site:securityweek.com claude-code 2026"` to the blog/practitioner discovery pass each run. SecurityWeek is the most consistent English-language source for Claude Code security research at practitioner depth. Flatt.tech is responsible disclosure-quality research that lands 2-4 weeks before CVE publication.

57. **CwC Tokyo AM run June 10 should lead with Tokyo announcements pass (per Suggestion 51)** — Tokyo keynote ends ~8 PM JST = 11 AM UTC June 10. The June 10 PM digest (12-23 UTC slot) is the first to catch all announcements. But the June 10 AM slot also fires at 00:00 UTC (before the keynote), so the AM run on June 10 cannot get Tokyo content. The PM run on June 10 should have `WebSearch: "CwC Tokyo 2026 announcements"` as its FIRST WebSearch call before any normal source coverage. Potential 8-10/10 item if Mythos 1 ships or new CC primitive drops. This is the highest-expected-value digest for the next 2 weeks.

