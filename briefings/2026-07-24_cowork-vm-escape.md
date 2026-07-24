# Briefing: Cowork SharedRoot VM Escape — CVE-2026-46331

**Score**: 7/10 · **Run**: 2026-07-24 PM · **Build-worthy**: FALSE (INFORMATIONAL + action item)

---

## What is it?

A VM escape vulnerability in Cowork's SharedRoot container runtime that allowed a malicious Cowork skill to break out of its sandbox and:
- Read any file under `~/` on the host
- Write files to the host filesystem (no root needed on macOS due to `~/Library` trust)

**CVE**: CVE-2026-46331  
**Root cause**: `act_pedit` TC action bypass in SharedRoot before skill sandboxing engaged  
**Affected**: ~500,000 macOS users who ran Cowork with local skill execution between July 1-22  
**Status**: PATCHED July 22. Cowork now defaults to cloud execution (remote sandbox) for all skills; local execution requires explicit `--local` flag.

---

## Why you'd want to know this

If you ran any third-party Cowork skills (`.claude/skills/`) between July 1-22 on macOS, treat the host as potentially compromised. Any secrets stored in `~/` were readable by any skill that ran in that window.

---

## Why I want it (Jarvis angle)

Step 4.5 (never auto-install third-party skills) exists for exactly this. The ExcellentPrompts `/fiction-mode` skill from the July 22 PM digest, for example — if you had installed that before the patch, it could have read everything in `~/`. Jarvis's policy of deep-dive-before-install is validated.

Also: this is what "cloud execution by default" means at the infra level. The fix was moving to remote sandbox — the same decision that makes Jarvis's CCR runs safe from local file reads.

---

## What to do

1. **Check skill install dates**: if you installed any third-party `.claude/skills/` before July 22, list them with `ls -la ~/.claude/skills/` and check modification dates
2. **Rotate `~/.anthropic` API key** as a precaution — it lives in `~/` and would have been readable
3. **Verify your Cowork version** is post-July 22 (check `cowork --version` or look for the cloud-default behavior)
4. **No action needed for first-party skills** (Anthropic's own Cowork skills were not exploit vehicles)

---

*Jarvis · 2026-07-24 PM*
