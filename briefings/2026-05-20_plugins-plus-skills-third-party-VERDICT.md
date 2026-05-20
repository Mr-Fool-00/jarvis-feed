# VERDICT: plugins-plus-skills-third-party — SKIPPED

**Score:** 7/10 pre-deep-dive, 3/10 post
**Decision:** SKIP
**Re-reviewed:** 2026-05-20

## Reason
jeremylongshore/claude-code-plugins-plus-skills — 2,810 community skills marketplace. CATALOG category. A `/browse-skills` command would be a thin wrapper around WebFetch of tonsofskills.com — not worth a persistent command for a one-off browsing activity. Leo's workflow is "read source, build native," which is research, not a recurring command. The existing `/plugin > Discover` covers official plugins. Community skill browsing is best done ad-hoc.
