# Briefing: Claude Code Embedded Hidden Unicode Markers in System Prompts for Chinese-Timezone Detection (CC v2.1.91–2.1.196)

**Score:** 8/10 · **build_worthy:** ❌ NO (informational)
**Source:** HN #48734373, Malwarebytes, CNBC · July 7–9, 2026
**URLs:**
- https://news.ycombinator.com/item?id=48734373
- https://www.malwarebytes.com/blog/news/2026/07/claude-codes-hidden-tracker-was-an-experiment-says-anthropic
- https://www.cnbc.com/2026/07/08/china-anthropic-ai-claude-code-backdoor-security-threat.html

---

## What happened

Researchers (LegitMichel777 / Thereallo) discovered that Claude Code versions 2.1.91 through 2.1.196 contained a hidden function in the minified JS bundle. The function:

1. Detected if the user's system timezone was `Asia/Shanghai` or `Asia/Urumqi`
2. If yes, mutated every outgoing system prompt by inserting invisible Unicode characters (zero-width spaces / Unicode steganography) at specific positions
3. These markers were presumably readable server-side to flag "likely China-based user"

This ran from approximately April 2 to July 1, 2026 — about 3 months. Fix shipped in v2.1.197.

---

## Anthropic's explanation

An Anthropic engineer confirmed the feature, describing it as "an experiment" designed to combat unauthorized resellers and capability distillation — specifically named in context of Anthropic's claim that Alibaba was extracting Claude's capabilities without authorization by routing Chinese users through unauthorized proxies.

The reasoning: by marking prompts originating from Chinese timezones, Anthropic could identify whether outputs were coming from legitimate Claude.ai sessions or from distillation pipelines using stolen API access.

---

## What actually happened as a result

**China's Ministry of Industry and Information Technology (MIIT)** issued a formal security alert on July 8 calling it a "backdoor," advising all organizations to uninstall CC versions 2.1.91–2.1.196 immediately or upgrade. The framing as "backdoor" is disputed by Anthropic (they say it's anti-abuse telemetry, not attacker-controlled access), but the functional description — hidden code that silently modifies prompts based on user timezone — is accurate.

**Alibaba** banned Claude Code for all employees effective ~July 10.

**HN** had the "Anthropic losing goodwill" thread (#48803751) hit the front page, combining this story with the Fable 5 nerfing backlash and the age/identity verification policy.

---

## What this means for Leo

**If you're on CC 2.1.197 or later: the tracker is not present.** Nothing to do technically.

The broader context: Anthropic ran a 3-month silent telemetry experiment inside the CLI tool that Leo (and millions of other developers) uses every day, without disclosure. It was a specific experiment targeting a specific geography for an anti-abuse reason, not mass surveillance. But it was undisclosed and it did modify user prompts.

This is worth knowing for the "who controls the tools you depend on" mental model — not a reason to abandon CC, but worth tracking as trust data.

---

## SecurityWeek supply-chain attack (related story)

Also in this news cycle: SecurityWeek reported researchers demonstrated a supply-chain attack where malicious-looking-innocent repositories trigger Claude Code to execute attacker-controlled commands on developer machines. Separate from the steganography story but compounded the security-focused coverage. No action needed beyond standard "don't open repos you don't trust in CC auto-mode."
