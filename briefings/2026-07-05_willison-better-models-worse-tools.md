# Briefing: "Better Models: Worse Tools" — Tool Schema Adherence Regression

**Item:** Simon Willison: "Better Models: Worse Tools"  
**Source:** simonwillison.net, July 4, 2026  
**URL:** https://simonwillison.net/2026/Jul/4/better-models-worse-tools/  
**Score:** 7/10 · **Digest:** 2026-07-05_PM · **Run:** PM  
**Action required:** Audit MCP tool schemas in Jarvis skills for strict-adherence gaps

---

## The finding

Opus 4.8 and Sonnet 5 — the two models you run daily in Claude Code — are calling tool APIs with **invented extra fields** that don't match the declared JSON schema.

Willison's specific example: Pi's edit tool. The model adds fields in nested arrays that were never defined in the schema. The model isn't making a mistake in its own view — it's "helpfully" extending the schema based on what it infers the tool *should* accept.

**This is a silent failure mode.** If your tool parser is strict, the call fails without an error message. If it's permissive, the spurious fields pass through to unpredictable behavior. Either way, you get silent wrong output rather than an explicit error.

---

## Why this is a regression (not a model improvement)

Earlier models (Sonnet 4.6 and earlier) adhered more closely to declared schemas. The newer models have internalized richer mental models of what tools are "for" — and that mental model causes them to extend schemas rather than honor them.

Willison's framing: "smarter but more opinionated." The model's tool-use capability improved in generation quality but regressed on specification adherence.

---

## Affected patterns in the Jarvis stack

Any MCP tool or custom skill in your stack that:
1. Takes a structured JSON input (tool call, skill invocation, hook payload)
2. Is called by Opus 4.8 or Sonnet 5 (which is: all of them, since those are your default models)

...is potentially affected.

Specifically at risk:
- **Jarvis skills with structured input schemas** — if a skill takes `{chapter: int, approach: string, context: object}`, Sonnet 5 may add a `notes` field or a `metadata` subobject
- **MCP tools with nested array schemas** — Willison's specific failure was in a nested array; tree-structured schemas are more vulnerable than flat ones
- **Hook payloads** — if hooks pass structured data to CC that CC then processes with a model, the model may extend the payload schema before returning

---

## Diagnostic steps

**Step 1: Identify at-risk tools.** List all MCP tools and skill schemas in your current stack that take structured (non-free-text) input.

**Step 2: Enable raw tool call logging.** Add debug-level logging that captures the exact tool call payload before parsing. One week of this will reveal whether Sonnet 5/Opus 4.8 are adding spurious fields in practice.

**Step 3: Add `additionalProperties: false` to schemas.** For each tool where strict adherence matters, add this to the JSON schema. This causes the model to surface an error rather than silently extend. Note: this may cause retry loops — the model will try different extensions before giving up. Monitor for retry spirals.

**Step 4: Test explicitly.** For your two most-used MCP tools, write a test that calls the tool via Sonnet 5 and Opus 4.8 and validates that the raw call payload matches the declared schema exactly. Run this once, then add it to periodic regression testing.

---

## The workaround tradeoff

Adding `additionalProperties: false` is the clean fix — but it has a side effect: when the model generates a non-compliant call, you get an error and retry loop rather than silent failure. This is actually better (you know something went wrong) but may cause temporary throughput reduction in automated pipelines.

For Jarvis sessions where human review is in the loop, prefer strict schemas with visible errors over permissive schemas with silent failures.

---

## Recommended immediate action

1. Audit the 3 most critical Jarvis skill schemas for `additionalProperties: false` compliance
2. Add raw tool call logging to the next Jarvis session and check for spurious fields
3. Add a note to AGENT_RUNBOOK.md: "Opus 4.8 and Sonnet 5 may add undeclared fields to tool calls — use strict JSON schemas with additionalProperties: false for production MCP tools"
