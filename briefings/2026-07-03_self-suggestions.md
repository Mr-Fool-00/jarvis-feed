# Self-suggestions — 2026-07-03 PM run

Two source gaps spotted this run that have been mentioned in prior suggestions but remain unactioned.

---

## 1. Add addyosmani.com/blog as a tracked WebSearch source

**What it is:** Addy Osmani's personal blog (addyosmani.com). He publishes 2-3 high-signal Claude Code / agent architecture posts per month, including posts this week on agentic code review and agent team swarm patterns.

**Why you'd want it:** His posts consistently score 7-8/10 for Leo's stack. They're already in seen.json from prior runs (June 28 PM, June 26 PM) but were found via accident, not a configured source. Adding a targeted WebSearch query would catch them reliably on publication day.

**Why I want it:** This has been suggested in entries #10 (May 22 AM), #52 (June 3 PM), and again here. At three logged misses it's a confirmed gap.

**What to do:** Add to SOURCES.yaml under the blog/practitioner discovery pass:
```
- name: "Addy Osmani blog"
  websearch: "site:addyosmani.com claude OR agent 2026"
  cadence: every_run
```

---

## 2. Expand arXiv window from 7 days to 30 days

**What it is:** The arxiv hours_window in SOURCES.yaml is currently 168 hours (7 days). Papers like AutoMem (2607.01224) and Loom (2607.00009) from this run were published July 1-2 and appeared immediately. But papers from May-June keep appearing as new because the community discussion around them (HN threads, blog posts) surfaces weeks after publication.

**Why you'd want it:** A 30-day window catches the "discussion wave" papers — ones that didn't get immediate HN attention but are now being cited and discussed in practitioner posts. It prevents the recurring pattern of surfacing a 6-week-old paper as if it's new.

**Why I want it:** Suggestion #13 (May 22 PM) and #64 (June 6 PM) both called for this. Papers worth surfacing are rarely older than 30 days by the time they make it into practitioner discussions.

**What to do:** In SOURCES.yaml, change:
```yaml
arxiv:
  hours_window: 168
```
to:
```yaml
arxiv:
  hours_window: 720
```
