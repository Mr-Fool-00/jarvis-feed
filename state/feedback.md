# Feedback log

Leo's reactions to past digest items. The agent reads this every run to refine ranking.

## Format

One reaction per line:
```
<YYYY-MM-DD> <item-id> <👍|👎|🤷> <one-line note about why>
```

Examples:
```
2026-05-19 reddit:t3_xyz 👍 finally a real production-scale agent post — more like this
2026-05-19 hn:39482001 👎 generic listicle, didn't read past title
2026-05-19 github:foo/bar 🤷 cool but not relevant to my stack
2026-05-20 rss:simonwillison:post-slug 👍 simon's takes always land for me, boost his weight
```

## How the agent uses this

- 👍 items: boost score (+1) for future items with similar topic/source
- 👎 items: penalize (-2) for similar
- 🤷 items: ignore (neutral signal, but logs that the agent surfaced something off-target)

## Tuning principles

- Be specific in your note — "boring" tells the agent nothing; "another LangChain hello-world tutorial" tells it to penalize that exact pattern.
- After ~14 days of feedback, the agent's ranking should sharpen noticeably. If it's not improving, the runbook ranking logic may need tightening.
- Feedback older than 30 days is still in the file but the agent weights it less.

---

(Empty — fills up as Leo tags items.)
