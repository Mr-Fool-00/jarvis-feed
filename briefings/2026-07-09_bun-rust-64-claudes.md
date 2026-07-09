# Briefing: Rewriting Bun in Rust with 64 Claudes — 11 Days, $165K, Passed All Tests

**Score:** 9/10 · **build_worthy:** ✅ YES
**Source:** Simon Willison · simonwillison.net · July 8, 2026
**URL:** https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/

---

## What happened

Simon Willison documented a project where Jarred Sumner (Bun's creator) ported the entire Bun JavaScript runtime from Zig to Rust using 64 parallel Claude instances running for 11 days. Total cost: ~$165K. Result: the Rust port passed all existing Bun test suites on day 11.

This isn't a toy benchmark — Bun is a real production runtime with a complex codebase. The fact that 64 parallel agents could carry out a multi-week port of production C-adjacent systems code and pass the existing test suite is the headline.

---

## The two patterns worth stealing

### 1. Dynamic fleet sizing via config

The agent fleet isn't hardcoded. A single `FLEET_SIZE` constant in `/config` controls how many Claude instances spin up, and the orchestrator adjusts task parallelism to match. Want to run cheaper? Set `FLEET_SIZE=8`. Parallelism saturated? Bump to 128. No architectural changes — just a config value.

The practical implication: you can build the same orchestrator pattern and run it at whatever scale your budget supports on any given day.

### 2. Adversarial-per-struct review

Every Rust struct definition went through this loop:
1. Writer Claude drafts the struct (fields, types, derives)
2. The draft goes to a separate Reviewer Claude with a single instruction: "Find a way this struct's invariants can be violated"
3. Reviewer returns either "clean" or a specific violation scenario
4. If violation: Writer revises, loop repeats until clean
5. If clean: struct commits

This is adversarial review at the unit of a struct definition — finer-grained than file-level code review, coarser than line-level. It caught subtle things: field ordering that implied ordering guarantees that weren't enforced, missing `#[non_exhaustive]` on enums that were meant to be private API surface, Clone derives on types that contained non-Clone-safe handles.

The orchestrator for this loop is straightforward — roughly 30 lines of agent orchestration code: spawn writer, spawn reviewer, compare outputs, loop on disagreement, commit on consensus.

---

## What to build from this

The SOP for Jarvis or any multi-agent pipeline:

**Pattern A — Dynamic fleet for parallel tasks**: When you have N independent subtasks (N > 5), don't spawn N hardcoded agents. Build an orchestrator that reads `FLEET_SIZE` from config, queues all tasks, and processes them `FLEET_SIZE` at a time. This is just a semaphore over your agent calls.

**Pattern B — Adversarial review for high-stakes outputs**: For any output where correctness matters (a schema, an API contract, a database migration, a CLAUDE.md rule), run it through a dedicated adversarial agent whose only job is "find a way this fails." One cycle of adversarial review catches what single-pass review misses — not because the reviewer is smarter, but because the incentive is reversed.

You can combine both: fleet of writers producing N outputs in parallel, each output going through an adversarial reviewer in parallel. Total latency = longest single write+review cycle, not sum of all cycles.

---

## Cost note

$165K for 64 instances × 11 days sounds like a lot. Per day: ~$15K. Per instance per day: ~$235. For porting an entire production runtime. For Leo's scale (single-project Jarvis runs), the same patterns at `FLEET_SIZE=4` for a 2-hour run would be well under $50.

---

## Source credibility

Simon Willison is the highest-signal AI engineering voice in the space — not a hype account, writes precise technical posts. This post includes the actual architecture details and cost breakdown, not just the headline.
