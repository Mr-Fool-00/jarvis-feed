# jeremylongshore/claude-code-plugins-plus-skills — 7/10 · ⚠️ Third-party code

## What it is
The biggest open-source Claude Code skill collection: 425 plugins, 2,810 skills, 200 agents. Has its own package manager (ccpi) and a searchable marketplace at tonsofskills.com. MIT licensed. Most of it is markdown SKILL.md files (prompts that guide Claude's behavior) — lower-risk than executable TypeScript plugins, but still third-party content. Updated daily via GitHub Actions.

## Why you'd want it (specific to your stack)
Your Jarvis skill library currently has ~10-15 skills. This is a reference library of 2,810. Even if you only use 20 of them as inspiration, that's a significant acceleration for your summer build window. Specifically worth checking: any writing/publishing-adjacent skills (chapter review, voice consistency, formatting), research skills (for the discovery loop), and orchestration patterns.

## Why I think it's worth your attention
It's basically a crowd-sourced skill database. The quality varies — some are "hello world" level, some are production-grade. Useful as a browsing resource for ideas, not as an install-everything drop.

## What I will do (safety rule)
I won't install any of these skills directly. If you want me to build a native version of any specific skill from this collection, tell me which one and I'll read the source, assess it, and build a Leo-owned version we understand and control. The ccpi package manager itself is third-party code — don't install it without a full review.

React 👍 to have me pick the 5 most relevant skills and write native versions for your stack. React 👎 to skip it entirely.

🔗 https://github.com/jeremylongshore/claude-code-plugins-plus-skills
🔗 https://www.tonsofskills.com
