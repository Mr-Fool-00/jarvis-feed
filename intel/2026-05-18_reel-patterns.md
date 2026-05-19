# Reel-Synthesis Intel — 2026-05-18

Synthesis of 21 Claude/AI Reels Leo ingested tonight via IG → yt-dlp → whisper. Identifies the creator playbook (so Leo can replicate it), surfaces the repos/products referenced (so Leo can install/use them), and notes the income models implied (so Leo can pick a money path).

---

## The Universal Reels Creator Playbook

Every successful Claude-tooling Reel in this batch follows the same 5-beat structure:

1. **Hook** — clickbait or problem statement. Examples: "Claude lies to you 17% of the time," "Is Claude Code DEAD?", "5 Claude skills every beginner needs," "POV: you built Jarvis to run your life," "Ranked #1 on GitHub right now."
2. **Reveal** — show the actual product or skill with a demo screenshot in the first 5–10 seconds. The screen capture IS the proof.
3. **Authority signal** — GitHub stars ("18.6k stars", "91,000 stars", "90,902"), named experts ("Karpathy made this", "Oli Lehman built this"), claimed results ("$4 million on options").
4. **Use-cases / specific applications** — 3–5 named examples of what someone could DO with the thing. NOT abstract benefits, concrete actions.
5. **CTA with comment keyword** — "Comment [WORD] and I'll DM you the file/setup/full guide." Captures the audience as DMs which auto-convert to email list signups.

**Keywords observed in this batch:** `Skills`, `Council`, `Brain`, `Learn`, `SKILL`, `TODO`, `Flow`. Each is a one-word trigger for a specific lead magnet.

**The economic engine underneath:** comment-bait → DM → opt-in to email list → sell course/community ($50–$500 product). The Reel itself doesn't make money; the *list* does. Every creator in this batch is running this exact funnel.

---

## Skills / Repos / Products referenced (intel for installation)

### Skills (Claude skill format)
- **Council** by Oli Lehman — 5-advisor panel (Contrarian / First Principles / Expansionist / Outsider / Executor) + peer review + chairman synthesis. Solves single-LLM yes-man problem. CTA keyword: `Council` or `SKILL`.
- **stop-slop** — humanizes Claude's writing, strips em-dashes and filter phrases. Directly relevant to Leo's writing pipeline.
- **marketing-skills** by Corey Haynes — 23 marketing sub-agents (SEO audits, copywriting, email sequences).
- **UI/UX Pro Max** — 50 UI styles + 99 UX guidelines built in.
- **remotion** — Claude opens its own video editor and builds animated production-ready videos from one prompt.
- **context engineering** — reduces tokens per Claude response, extends Max plan headroom.

### Repos
- **karpathy/llm-council** (18.6k★) — open-source implementation of the Council pattern. The canonical multi-advisor reference. Karpathy.
- **ruvnet/ruflo** ("ranked #1 on GitHub", 60+ AI agents that self-improve, shared collective memory, routes simple→cheap/complex→powerful, claims 50% token reduction + 250% Claude Code extension). Worth a serious look — directly attacks Leo's cost-per-chapter problem.
- **breferrari/obsidian-mind** (10 tags, .claude-plugin / .codex / .gemini / .shardmind structure) — Claude + Obsidian "unlimited memory" pattern, public.
- **"147 agents" repo** (91k★) — 91,000 stars, install all 147 pre-built agents with one shell command. Includes tax strategist, AI citation strategist (optimizes brand for being cited in ChatGPT/Claude/Gemini/Perplexity answers), and a "fractional CTO" agent.

### Products / Tools
- **Hermes Agent** by Nous Research — fastest GitHub project in history to 100k stars. Server-resident, text-from-anywhere, self-improving (writes new skills for itself after each task). Pair with **Browser Harness** (lets the agent see/click/type on any website, also self-improving). Lives at `hermes-agent.nousresearch.com` per the landing page screenshot.
- **OpenClaw** — agent dashboard with config/source-code/preview tabs, generates agent YAML from prompts (e.g. "Build an agent that evaluates acquisition targets" → outputs config + sample API call). Competitor positioning vs Claude Code.
- **n8n** — visual workflow automation, paired with OpenClaw in the M&A analyst demo.
- **Higgsfield Supercomputer** — product launch (Apple-launch-style framing, Steve Jobs cuts in the Reel).
- **OmniSocials** — MCP server that lets Claude post to your real IG/LinkedIn/TikTok/X/Threads/Mastodon accounts. The OUTBOUND companion if Leo commits to a content account.
- **Anthropic Console agent dashboard** — `claude-opus-4-6` model selector with `agent_toolset_2026-04-01` tools (visible in the n8n+OpenClaw demo).

---

## Architecture themes (technical patterns worth studying)

1. **Multi-agent orchestration** — Council, ruflo, 147-agents, n8n. The pattern is: many specialized agents > one big agent. Each has a narrow role, they share memory/state.
2. **Persistent agent memory** — Obsidian-mind, Hermes ("never forgets"), ruflo ("collective memory"). The promise: agent gets *better* with use, not worse.
3. **Browser/computer control** — Hermes + Browser Harness, the Anthropic computer-use API (visible in the "Claude pays the bills" Reel that mentioned screenshot capture, mouse control, keyboard input, desktop automation). This is the "agent does things on real websites" wave.
4. **Smart model routing** — ruflo routes simple→cheap (Haiku), complex→powerful (Opus). Same idea used in Leo's existing pipeline split — but more aggressive. Token savings claimed: 50%.
5. **Skill / Plugin architecture** — every modern repo has `.claude-plugin/`, `.agents/`, `.claude/`, `.githooks/`, sometimes `.codex/` and `.gemini/`. Multi-LLM compatibility is becoming standard.

---

## Income models implied (Leo's monetization paths)

Ranked by speed-to-first-dollar:

| Model | First $ | Effort | Risk | Notes |
|---|---|---|---|---|
| **Skill on Gumroad/Whop** ($5–15) | 1–3 days | Low | None | Package an existing skill (stop-slop equivalent, your fixer chain, voice DNA system). Promote via 1 Reel. |
| **Comment-bait → email list → course** | 1–2 weeks | Medium | None | The exact playbook above. Build list first, sell later. |
| **Freelance Claude Code consulting** | 2–7 days | High (sales) | Low | Fiverr/Upwork. Your existing pipeline is the deliverable. |
| **AI ghostwriting** | 1–14 days | Medium | Low | $50–200/chapter via your Kindle pipeline. |
| **Self-publish to KDP** | 4–12 weeks | High (writing) | Low | Long game, but truly passive once shipped. |
| **SaaS micro-product** | 4–8 weeks | High (eng) | Medium | Jarvis Discovery Loop as a service for others. |
| **Content account (Reels)** | 8+ weeks | High (consistency) | None | AdSense + sponsorships. Compounding asset. |

---

## Direct relevance to Leo's existing stack

| Leo's existing work | This batch's insight |
|---|---|
| Kindle longform pipeline (40+ ch/book) | ruflo's cost-routing pattern could cut your $/book by 50%. Worth studying their model-selector logic. |
| Fixer chain (multi-pass quality) | stop-slop skill is a drop-in addition to your fixer chain — install + measure delta. |
| Voice DNA system | breferrari/obsidian-mind shows a clean Obsidian+Claude memory pattern for cross-chapter consistency. Possible upgrade path from your current file-based system. |
| Council-style brainstorming | Karpathy's llm-council repo is THE reference. Install it and run your "should I do X?" decisions through it. |
| Cost discipline (Max plan) | 147-agents repo + ruflo's routing both target the same constraint. Worth installing one and measuring impact on weekly Max bucket. |
| Future writing-subsystem (Jarvis north star) | Hermes Agent is the closest existing implementation of "text-from-anywhere → autonomous server agent that learns." Don't copy it — but the architecture (server-resident, self-improving via written skills, browser harness) is the destination. |

---

## What's NOT here (gaps worth filling on the next ingest)

- No Reels demonstrating actual revenue numbers from any of these creators — would help calibrate the income model EVs
- No Reels covering Anthropic's own product roadmap (model releases, new features)
- Limited coverage of self-hosted / local LLM options (one Hermes mention)
- No coverage of agent-to-agent protocols (A2A, MCP-to-MCP) which are emerging
- No coverage of agent evaluation / benchmarking practices

These are good search seeds for the next Jarvis run via YouTube channels (AI Explained covers Anthropic roadmap, Cole Medin covers agent eval).

---

## Recommended action items (prioritized)

1. **Install Council skill on your Claude Code.** Comment "Council" on Oli Lehman's Reel (DXZlBrtCkC2) or check github.com/karpathy/llm-council directly. Use it on your next chapter-arc decision.
2. **Test ruflo's model-routing on one chapter.** If it really cuts tokens 50%, that's $5/chapter savings — material at scale.
3. **Try stop-slop on your existing fixer chain.** Cheap test, drop-in addition.
4. **Pick one money path from the income table above and commit.** Path A (Gumroad skill) is still the fastest.
5. **When ready: build a Reel using THIS playbook** — your stop-slop-equivalent / voice-DNA / fixer-chain skill, comment-bait CTA, drive to your own Gumroad / community.
