# Agent Cookbook — paste-ready prompts for full Alethia demos

Every block below is a **literal prompt**. Paste it into Claude, Cursor, Cline, or any MCP client that has Alethia configured, and the agent will orchestrate the tool chain itself. No tool names, no JSON — that's the point of Alethia.

**Prerequisites**

- `@vitronai/alethia` configured as an MCP server in your agent client. See [quickstart-mcp.md](quickstart-mcp.md) for 30-second setup per client.
- The Alethia cockpit opens by default — you'll see each step highlighted live (green = pass, blue = type, red = EA1 block).

---

## Demo 1 — Smoke test a local app

**Goal:** Drive a running localhost app with plain English and confirm the happy path.

**Prompt to paste:**

```
Use Alethia to smoke test my dev server at http://localhost:3000. Navigate to it, confirm the page loads, and run a quick sign-in flow with email "demo@example.com" and password "demo". Tell me what worked and what didn't.
```

**What the agent does:** one `alethia_tell` call with the full NLP script. Returns per-step timings, DOM diffs, and a semantic page snapshot.

---

## Demo 2 — Bootstrap test coverage on an unknown page

**Goal:** Point Alethia at a page you've never tested and get a ready-to-run NLP test suite back.

**Prompt to paste:**

```
Scan http://localhost:3000/settings with Alethia, generate a test suite for everything interactive on the page, and run it. Include destructive-action safety checks — I want proof the EA1 gate blocks anything that could wipe data.
```

**What the agent does:** `alethia_propose_tests` returns a candidate suite (including auto-generated `expect block:` lines for destructive actions). The agent then calls `alethia_tell` to run each block and reports which passed, which failed, and which were correctly blocked.

---

## Demo 3 — Full compliance audit (WCAG + NIST) with signed evidence

**Goal:** Run accessibility + security audits on a page and export a cryptographically-signed evidence pack for compliance review.

**Prompt to paste:**

```
I need a compliance audit on http://localhost:3000/checkout using Alethia. Run WCAG 2.1 AA, then NIST SP 800-53, then export the full session as a signed evidence pack. Summarize the findings by severity and tell me the SHA-256 hash of the evidence.
```

**What the agent does:** `alethia_tell` (navigate) → `alethia_audit_wcag` → `alethia_audit_nist` → `alethia_export_session`. The evidence pack is SHA-256 hashed and includes every tool call, input, output, policy decision, and timestamp from the session.

---

## Demo 4 — Prove the EA1 safety gate works on a real page

**Goal:** Walk every destructive action on a page and confirm each one is refused by the policy engine. This is the automated compliance primitive.

**Prompt to paste:**

```
Use Alethia's safety assertion tool on http://localhost:3000/admin. Discover every destructive action (delete, purge, reset, transfer) and verify the EA1 gate blocks each one. Any action that isn't blocked is a safety regression — flag it loudly.
```

**What the agent does:** `alethia_assert_safety` auto-discovers destructive buttons, attempts each, and returns a per-action `blocked: true/false` report. The agent surfaces any `blocked: false` rows as regressions.

---

## Demo 5 — Parallel multi-page verification

**Goal:** Run the same smoke check across several pages concurrently.

**Prompt to paste:**

```
With Alethia, run a parallel smoke test across these three pages of my app:
- http://localhost:3000/login  — confirm the login form is present
- http://localhost:3000/dashboard — confirm the dashboard heading loads
- http://localhost:3000/settings — confirm the settings panel renders

Use parallel execution, not sequential. Summarize all three results together.
```

**What the agent does:** one `alethia_tell_parallel` call — each spec gets its own browser instance, all run concurrently.

---

## Demo 6 — Live partner walkthrough (cockpit-visible)

**Goal:** Demo Alethia to a partner on-screen. Pop the cockpit, run a visible flow with highlights, export signed evidence.

**Prompt to paste:**

```
I'm demoing Alethia live. Show the cockpit window, then drive the local demo server: use Alethia's built-in demo launcher to serve the admin-panel demo, navigate there, try to delete the first user (which should be blocked by EA1), then try a legitimate view-profile action (which should succeed). Export a signed evidence pack at the end — I want to show the SHA-256 hash and the policy decisions in the pack.
```

**What the agent does:** `alethia_show_cockpit` → `alethia_serve_demo` → `alethia_tell` (with `expect block:` for the delete) → `alethia_export_session`. Green highlights flash on successful steps, red on the EA1 block. The signed pack is the money-shot for compliance-minded partners.

---

## Tips for writing your own prompts

- **Say the goal, not the tool.** "Audit this page for accessibility" is better than "call alethia_audit_wcag." The agent picks the right tool.
- **State the URL once.** Alethia reuses the current page across tool calls; you don't need to re-navigate.
- **For destructive actions, phrase them as expectations.** "Attempt to delete the first record — I expect EA1 to block this" makes the agent use the `expect block:` pattern, which turns a safety refusal into a test pass.
- **End compliance-sensitive sessions with "export the evidence."** The signed pack is the artifact auditors and partners actually want.
- **If you want to watch each step flash on-screen**, say "show the cockpit first." Otherwise it's visible by default — set `ALETHIA_HEADLESS=1` to hide.

---

## What Alethia does NOT do

- **Does not touch non-local origins.** Anything outside `file://`, `localhost`, `127.0.0.1`, `.local`, or RFC1918 is refused at the main-process boundary. No env var, CLI flag, or MCP argument expands this. Design-partner production access is handled by a separately-signed build — see [vitron.ai/safety](https://vitron.ai/safety).
- **Does not write or execute destructive actions by default.** Delete, purchase, transfer, submit, and sensitive-input writes are refused unconditionally unless the calling NLP explicitly wraps them in `expect block:` (which asserts the refusal rather than bypassing it).
- **Does not send telemetry.** Zero cloud calls, zero analytics. Loopback JSON-RPC only.
