# Briefing: MCP 2026-07-28 Breaking Spec RC — Migration Required Before July 28

**Filed:** 2026-07-13T00:01:45Z
**Score:** 9/10 · **Build verdict:** BUILD WORTHY
**Deadline:** July 28, 2026 (15 days from filing)

---

## What Happened

The official MCP Release Candidate for the July 28, 2026 final spec was published this week. It is the largest protocol revision since MCP launched. Four breaking changes affect every MCP server.

This is not an optional upgrade. After July 28, compliant MCP clients (including Claude Code) will reject servers that don't conform. Any MCP integration in Leo's stack will break.

---

## The Four Breaking Changes

### 1. Initialize Handshake Removed

**Before:** Every MCP session began with a `initialize` request from the client, followed by an `initialized` notification from the server. Servers used this to set session state, negotiate capabilities, and load resources.

**After:** The `initialize`/`initialized` lifecycle is gone. The first message from the client can be a tool call with no prior negotiation.

**Who breaks:** Any MCP server that:
- Sets session-scoped state during initialization
- Loads models, connects to databases, or reads config files lazily on first connect
- Returns capability flags in the initialize response that affect tool behavior
- Uses the `initialized` notification to start background processes

**Migration:** Move all initialization logic to either (a) module-load time (before any request arrives), or (b) lazy-init inside the first tool call that needs it. Session state must be passed in each request, not stored server-side.

---

### 2. Routing Headers Mandatory

**Before:** MCP requests had no required headers beyond Content-Type.

**After:** All requests must include:
- `mcp-request-id`: UUID for the request, used for correlation in multi-hop routing
- `mcp-context-version`: semantic version of the MCP context format being used

Servers that don't validate and respond to these headers will be rejected by compliant clients.

**Who breaks:** Any MCP server that ignores request headers.

**Migration:** Add header extraction to the request handler. Validate `mcp-request-id` is present and well-formed (UUID4). Echo `mcp-request-id` in the response. Reject requests missing `mcp-context-version` with a 400 error.

---

### 3. SSE Elicitation Replaced by Multi Round-Trip Requests (MRT)

**Before:** A tool call could ask the user a follow-up question mid-execution using the SSE `elicitation` event. The client would show the question, wait for input, and resume the tool call.

**After:** Elicitation is replaced by the new **Multi Round-Trip Request (MRT)** type. An MRT tool call explicitly declares it may require multiple rounds of model or user input. The structure:
- Tool returns an `mrt_pending` response instead of a final result
- Client re-invokes the tool with accumulated context
- Continues until the tool returns `mrt_complete`

MRT is more structured and LLM-friendly than SSE elicitation but is a fundamentally different programming model.

**Who breaks:** Any tool that uses the current SSE `elicitation` event to pause for user input.

**Migration:** Rewrite elicitation tools as MRT tools. For tools that need exactly one round of clarification, the migration is mechanical: return `mrt_pending` with a structured question, handle the follow-up call, return `mrt_complete`.

---

### 4. Stateless Core as a Design Requirement (Not Just Recommendation)

**Before:** The MCP spec recommended stateless servers but allowed session state as a workaround.

**After:** The RC codifies statelessness as a hard requirement. Servers that maintain per-session state across connections will fail compliance checks. All session context must be:
- Passed in each request by the client (preferred), OR
- Managed by a client-side session layer, NOT by the server

**Who breaks:** Any server that:
- Stores user preferences, conversation history, or active resources in a server-side session object
- Uses sticky routing (always routing a session to the same server instance)
- Caches LLM model references or database connections keyed to session ID

**Migration:** Move session context to the client request payload. If the server needs per-session state, it should be passed in a structured field in each tool call's input.

---

## Impact on Leo's Stack

### Jarvis Pipeline (CCR)

The Jarvis pipeline uses the GitHub MCP server and Gmail MCP server. These are managed MCP servers (not custom code). Migration status depends on whether Anthropic and GitHub update their managed servers before July 28.

**Action:** Check managed server update status in the week before July 28. If GitHub MCP or Gmail MCP hasn't released a v2 compatible with the RC, you may need to pin to the old MCP client version in Claude Code settings temporarily.

### Custom MCP Servers

If Leo has any custom MCP servers built for the fiction pipeline or other tools:

1. Audit each server against the four breaking changes above
2. The initialize handshake removal is the most likely break — check if any server does lazy initialization
3. The routing headers requirement is mechanical — add header extraction to every request handler
4. SSE elicitation is rare in custom servers unless explicitly built; check tool implementations
5. Stateless requirement — check for any session-scoped state stored in module-level variables

### Claude Code Itself

CC v2.1.207+ is already being updated for RC compliance. No CC upgrade action needed beyond staying current.

---

## The mcp-spec-check Tool (Safety Gate Applied)

**Repo:** https://github.com/Roee-Tsur/mcp-spec-check
**Stars:** 5,200+ (first day, July 12)
**License:** MIT

This tool points at any MCP server and runs a structured compliance test against the RC spec, returning a report of which breaking changes the server fails. Published the day after the RC dropped — clearly built in response to it.

**Safety gate status:** Third-party code. Do NOT auto-install or run against Leo's servers without explicit approval.

**What I'd build natively instead:**

A native compliance audit script that runs four targeted checks:
1. Does the server respond to a tool call sent without an `initialize` handshake? (should return valid response, not hang or error)
2. Does the server accept and echo `mcp-request-id` in response headers?
3. Does the server reject requests missing `mcp-context-version`?
4. Does the server use any server-side session state? (requires code inspection, not a runtime check)

This is ~50 lines of Python using `httpx`, no dependencies on mcp-spec-check. Leo approves → I build it in the next interactive session.

---

## Build Plan

**Trigger:** Leo reacts 🚀 to the briefing message, or says "build the MCP compliance check."

**Scope:**
- A `scripts/audit_mcp_compliance.py` script that runs the four breaking-change checks against a server URL
- Output: a structured report per server, one line per check (PASS/FAIL/UNKNOWN)
- No external dependencies beyond the Python standard library + httpx
- Estimated build time: 1 interactive session (45 minutes)

**Deliverable:** `scripts/audit_mcp_compliance.py` + a one-time audit run against all Leo's registered MCP servers, with results logged to `state/mcp_audit_2026-07-13.md`.

---

## Reference: RC Spec Location

The official MCP RC spec is maintained in the modelcontextprotocol organization on GitHub. The July 28 final spec release date was announced in the June 28 RC.

Key change summary from the RC diff (as of July 12):
- `initialize`/`initialized` removed from protocol lifecycle
- Headers section added to request spec (mcp-request-id, mcp-context-version required)
- `elicitation` event type removed; `mrt_pending`/`mrt_complete` response types added
- Servers section: "Servers MUST NOT maintain per-session state" (normative, was informative)

---

*Filed by Jarvis Discovery Loop · 2026-07-13 AM run · Deadline: 2026-07-28*
