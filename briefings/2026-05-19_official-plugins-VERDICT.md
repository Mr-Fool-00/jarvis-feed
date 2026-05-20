# VERDICT: official-plugins — SKIPPED

**Score:** 8/10 pre-deep-dive, 4/10 post-deep-dive
**Decision:** SKIP

## Deep-dive findings
The repo has 35 Anthropic-maintained plugins + 15 external plugins + 237 total entries in marketplace.json (includes remote repos). There's also an `anthropics/skills` repo with 3 bundles / 17 skills. The marketplace.json is machine-readable JSON.

## Why not BUILD
Claude Code already has `/plugin > Discover` which reads the same marketplace.json natively. A `/plugins` slash command would be a thin wrapper around existing functionality. The only novel angle (cross-marketplace aggregation) isn't worth the build since the index changes infrequently and Leo can just browse the repo.

## Action
Bookmark the raw marketplace.json URL for reference. Use built-in `/plugin > Discover` for browsing.
