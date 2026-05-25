# NVIDIA Verified Agent Skills — 8/10

## What it is

NVIDIA launched a security scanner for AI agent skills called SkillSpector — it scans any skill before you use it and flags hidden instructions, prompt injection attempts, tool poisoning, excessive permissions, and data exfiltration risks. They also started publishing a catalog of "verified" skills that have been scanned, signed with a cryptographic signature, and paired with a skill card explaining exactly what the skill does and what risks were found. The whole system is open-source and works with Claude Code (same SKILL.md format).

## Why you'd want it (specific to your stack)

Right now, every time Jarvis finds a third-party skill worth 7+/10, I have to manually read the README, check recent commits, and grep for red flags — because installing a malicious skill could compromise your whole setup. SkillSpector automates exactly that check. It specifically looks for the things I check manually PLUS agent-specific risks a human reviewer would likely miss (like a skill that says it's a "code formatter" but actually triggers extra tool calls or exfiltrates context). Running it before any skill adoption takes 30 seconds instead of 10 minutes, and it catches more. The open-source GitHub repo is NVIDIA/SkillSpector.

## Why I think it's worth your attention

Your manual safety gate process (the thing I do in the runbook before recommending any third-party code) is exactly what SkillSpector formalizes. The skills supply chain is now the attack surface. Three months ago this wasn't a named problem — this week it's NVIDIA's priority. You're already ahead of most practitioners on this; SkillSpector is the tool that matches the mindset.

## What to do

Look at github.com/NVIDIA/SkillSpector — it's open-source and documented. If you approve, Jarvis can build a `/check-skill <github-url>` command that clones a skill repo, runs SkillSpector against it, and returns a PASS/FAIL verdict before any adoption decision. That's a 30-minute build that permanently upgrades the safety gate workflow.

React 🚀 to approve the build. React 👍 to save it for later. React 👎 to drop it.

🔗 https://developer.nvidia.com/blog/nvidia-verified-agent-skills-provide-capability-governance-for-ai-agents/
📦 https://github.com/NVIDIA/SkillSpector
📂 https://github.com/NVIDIA/skills
