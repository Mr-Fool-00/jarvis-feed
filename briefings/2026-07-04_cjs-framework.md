# CJS Framework: Cyber Jailbreak Severity — Industry's CVSS Equivalent for AI Jailbreaks

**Source:** Anthropic Security Blog · Amazon + Microsoft + Google co-signatories · July 2026  
**Score:** 7/10 · **build_worthy:** FALSE (industry standard to be aware of, not a tool to build)

---

## What it is

The Cyber Jailbreak Severity (CJS) Framework is the AI security industry's answer to CVSS (Common Vulnerability Scoring System). It gives security teams, model providers, and enterprise risk functions a shared vocabulary for rating the severity of AI jailbreaks — the same way CVSS gave the software security industry a shared vocabulary for rating traditional CVEs.

Developed jointly by Anthropic, Amazon, Microsoft, and Google. Not an academic paper — this is a live operational framework already being used in production model safety stacks.

## The four scoring axes

| Axis | What it measures | Example: low | Example: high |
|------|-----------------|--------------|---------------|
| **Capability Gain** | How much does the jailbreak expand what the model can do beyond its policy? | Access to mild off-topic content | Access to weapons synthesis, cyberattack tooling |
| **Universality (Breadth)** | Does it work on one model, or across many? | Single-model, single-version | Cross-provider, works on GPT + Claude + Gemini |
| **Ease of Weaponization** | How hard is it to exploit? | Requires expert prompt engineering + multiple attempts | Single-sentence, works first try |
| **Discoverability** | How easy is it for attackers to find independently? | Requires deep model internals knowledge | Discoverable by naive users in normal conversation |

**Severity bands:** CJS-0 (Informational) → CJS-1 (Low) → CJS-2 (Moderate) → CJS-3 (High) → CJS-4 (Critical). Bands are exponential — a CJS-4 is an order of magnitude more dangerous than a CJS-3, not incrementally worse.

## The Mythos connection

The CJS Framework is the scoring backbone for Anthropic's "Mythos" cybersecurity classifier, which shipped inside Fable 5's safety stack. Mythos evaluates incoming prompts against the four CJS axes in real-time and gates Fable on CJS-3 and CJS-4 patterns.

This is what satisfied the export control condition that got Fable restored on July 1. The US government's line was: "if you can demonstrate real-time detection and blocking of high-severity jailbreak patterns, we'll lift the suspension." Mythos/CJS is that demonstration.

## Why it matters

**Direct:** If you run Jarvis subagents that do experimental prompt iteration or test-harness work, CJS is the vocabulary for categorizing what those experiments are touching. "This prompt pattern is a CJS-2 at most" vs. "this has CJS-4 universality properties" is a meaningful distinction.

**Broader signal:** CJS will become the standard taxonomy for AI security reporting the way CVSS became the standard for CVE reporting. Within 12 months, model provider security advisories, enterprise AI risk assessments, and bug bounty programs will cite CJS scores. This is the moment to learn the schema — it's simple, it maps to intuition, and it's going to be everywhere.

**Fable context:** Knowing that Fable 5 has a real-time CJS-3/4 classifier built in changes the mental model for what Fable can and can't do on creative fiction tasks. CJS-1/2 patterns (mild policy expansion, model-specific, hard to discover) are not Fable's concern. CJS-3/4 are. Fiction prompts that are vivid, dark, or morally complex are CJS-0/1 by this taxonomy — they don't gain capability, they don't work cross-provider, and they're not weaponizable. This is part of why Fable can be more permissive for creative fiction: the classifier distinguishes creative darkness from genuine harm-enablement.

## What to do

No action required today. Read the Anthropic Security Blog post when convenient — the full scoring rubric is worth 10 minutes. The Fable model selection decision for creative fiction tasks is already correct given this context.

Reference: Anthropic Security Blog, July 2026. Search: "Cyber Jailbreak Severity Framework Anthropic CJS"
