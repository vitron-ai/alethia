<div align="center">

# Alethia

**The patent-pending zero-IPC E2E test runtime built for AI agents.**

**45× faster than Playwright** on the localhost test loop.
Fail-closed by default. Cryptographically chained audit packs.
**Local-first. Zero telemetry by default. Opt-in cloud.** No CDP.

[![npm](https://img.shields.io/npm/v/@vitronai/alethia.svg?label=%40vitronai%2Falethia&color=1fd67f&logo=npm&logoColor=white)](https://www.npmjs.com/package/@vitronai/alethia)
[![License: MIT](https://img.shields.io/badge/MCP%20bridge-MIT-1fd67f.svg?logo=opensourceinitiative&logoColor=white)](https://github.com/vitron-ai/alethia-mcp/blob/main/LICENSE)
[![Patent Pending](https://img.shields.io/badge/Patent-Pending-5fb4f7.svg?logo=shield&logoColor=white)](#patent-notice)
[![Status](https://img.shields.io/badge/status-v0.3%20shipped-1fd67f.svg?logo=checkmarx&logoColor=white)](#status)
[![Tessl](https://img.shields.io/badge/Tessl-Registry-5fb4f7.svg?logo=data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjUgMjgiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PHBhdGggZD0iTTYuODcgMTIuMjJjMC0uMjUuMjctLjQxLjQ5LS4yOGw0LjQ0IDIuNTd2NS43OGMwIDEuMDEgMS4xIDEuNjMgMS45NiAxLjEybDQuMjYtMi41NHY2LjMybC00LjI3IDIuNDdjLS44MS40Ny0xLjguNDctMi42IDBsLTQuMjgtMi40N1YxMi4yMloiIGZpbGw9IndoaXRlIi8+PC9zdmc+)](https://tessl.io/registry/vitron-ai/alethia)

</div>

---

## Why Alethia exists

Your AI agent generates a Next.js app on `localhost:3000` and your users want it verified. Playwright adds **600ms of CDP marshalling tax** to every assertion. Worse, it's async-by-construction — the agent's decide-act-verify loop is racing against stale DOM snapshots between every `await`.

Alethia is a different architectural shape. The driver and the DOM live in **the same V8 isolate**. Reading `document.querySelector('button').textContent` is a function call, not a network round-trip.

| | Playwright (CDP) | Alethia (zero-IPC) |
|---|---|---|
| Avg latency per step | 580 ms | **13 ms** |
| p95 latency per step | 654 ms | **24 ms** |
| Process boundary | 3 (test ↔ driver ↔ browser) | **0** between driver and DOM |
| DOM access | async, marshalled, race-prone | **synchronous, in-process** |
| Per-step safety policy | none | **VITRON-EA1 fail-closed gate** |
| Audit trail | trace viewer (debugging) | **SHA-256 chained, Ed25519 signable** |
| Telemetry | on by default in cloud product | **off by default; opt-in only** |
| Patent moat | none | **U.S. App 19/571,437** |

Benchmark: `click-assert-wait` scenario, 20 iterations, full numbers in the [evidence pack](#evidence).

---

## Status

**Alethia v0.3 is shipping as of April 2026.**

- ✅ **MCP bridge** on npm: [`@vitronai/alethia`](https://www.npmjs.com/package/@vitronai/alethia) — auto-installs the headless runtime on first call
- ✅ **MCP bridge source** open and auditable: [vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp) — MIT
- ✅ **Cross-platform headless binaries**: macOS x64/arm64, Linux x64/arm64, Windows x64 — [GitHub Releases](https://github.com/vitron-ai/alethia/releases)
- ✅ **Ed25519-signed releases**: every binary is signature-verified before extraction
- ✅ **Patent filed**: U.S. Patent Application No. 19/571,437 (non-provisional, claiming priority to 63/785,814 filed April 9, 2025)
- ✅ **Headless mode**: agent-driven spawning with `--headless`, no visible window
- ⏳ **Public evaluation binary** (full runtime under 60-day eval license) — coming soon
- ⏳ **Cloud dashboard** (signed evidence as a service, team collaboration) — post-eval

**One npm install. Zero manual steps.** `npm install -g @vitronai/alethia` — the bridge auto-downloads the signed headless runtime on first use. For licensing inquiries, email **gatekeeper@vitron.ai**.

---

## How it works

```
┌────────────────────────┐
│  Your AI agent         │  Claude Code · Cursor · Cline · Continue · ...
│  speaks MCP stdio      │
└──────────┬─────────────┘
           │ stdio (newline-delimited JSON-RPC)
           ↓
┌────────────────────────┐
│  @vitronai/alethia     │  npm package, ~22 KB, MIT, source on github
│  stdio → HTTP shim     │  Auto-installs runtime on first call. Loopback only.
└──────────┬─────────────┘
           │ HTTP POST 127.0.0.1:47432 (loopback only, never networked)
           ↓
┌────────────────────────┐
│  Alethia runtime       │  Desktop runtime — proprietary, patent pending
│  local JSON-RPC server │  Loopback bind, never reachable from network
└──────────┬─────────────┘
           │ in-process JS bridge
           ↓
┌────────────────────────┐
│  Alethia renderer      │  Embedded browser — IS the browser
│  zero-IPC runtime      │  tell() → NLP compiler → Action IR
└──────────┬─────────────┘  VITRON-EA1 policy gate (per-step, fail-closed)
           │ direct synchronous DOM access
           ↓
┌────────────────────────┐
│  The page under test   │  Your localhost dev server, your file://, your app://
└────────────────────────┘
```

**Two process boundaries** between your agent and the runtime (agent ↔ shim, shim ↔ runtime). Then **zero** boundaries between the runtime and the DOM. That's the architectural difference that makes Alethia 45× faster than Playwright on the localhost test loop.

---

## Try it

### 1. Install

```bash
npm install -g @vitronai/alethia
```

That's it. The bridge auto-downloads the signed headless runtime on first use — Ed25519 verified, SHA-256 checked, no signup required.

### 2. Verify

```bash
alethia-mcp --health-check
```

```
✓ Connected. MCP tools available.
  runtime version:  0.1.0-alpha.4
  default profile:  controlled-web
  kill switch:      inactive
```

### 3. Configure your agent

Add to your MCP config (`.mcp.json`, Claude Code settings, Cursor MCP, Cline, etc.):

```json
{
  "mcpServers": {
    "alethia": { "command": "alethia-mcp" }
  }
}
```

### 4. Drive Alethia from your agent

> *"Use alethia_tell to navigate to file:///path/to/app.html, type test@example.com into the email field, click Sign In, and assert the dashboard is visible."*

The agent calls `alethia_tell` with that NLP. Alethia compiles it to Action IR, runs it through the VITRON-EA1 policy gate (default: `controlled-web`, fail-closed), executes step-by-step with synchronous DOM access, and returns a signed `PlanRun` with per-step results, policy audit records, and a SHA-256 integrity hash.

---

## MCP tools

| Tool | Purpose |
|---|---|
| `alethia_tell` | Execute natural-language test instructions. The headline tool. ~13 ms per step. |
| `alethia_compile` | Compile NL → Action IR without executing. Preview before you run. |
| `alethia_status` | Health + identity probe. Version, profile, kill switch state, driver stats. |
| `alethia_activate_kill_switch` | Halt all automation immediately. Optional reason logged in audit trail. |
| `alethia_reset_kill_switch` | Clear an active kill switch. Re-enables `tell()` calls. |
| `alethia_screenshot` | Capture a PNG screenshot of the current page. Visual verification for agent loops. |
| `alethia_eval` | Evaluate a JS expression in the page under test. Escape hatch for raw DOM queries. |
| `alethia_audit_wcag` | WCAG 2.1 AA accessibility audit — 14 criteria. Section 508 compliance. |
| `alethia_audit_nist` | NIST SP 800-53 security controls audit — 8 controls across AC, IA, SI families. |

Full docs: [npmjs.com/package/@vitronai/alethia](https://www.npmjs.com/package/@vitronai/alethia)

---

## VITRON-EA1: Ethical Automation v1

**The first browser automation framework with built-in runtime-enforced ethical guardrails.** Every step passes through a fail-closed policy gate before execution and produces a cryptographically chained audit record. The gate is in the same process as the executor — agents cannot bypass it.

Default classifications:
- **read** (navigation, scraping, assertions) — always allowed
- **write-low** (form input, drafts) — allowed by default
- **write-high** (delete, purchase, transfer, submit-payment) — **blocked by default**
- **blocked** (always denied)

The NLP compiler infers intent: *"delete the user account"* → `!://write-high CLICK :text(user account)`. *"type my password"* → `!://write-high TYPE` blocked unless explicit `allowSensitiveInput: true`.

VITRON-EA1 is positioned as a **publishable standard** — other automation runtimes will be able to claim conformance via Ed25519-signed audit records verified against a public key registry. The conformance scheme binds adoption to the patent license: registered conforming implementations must implement per-step in-process enforcement, which is the patented method.

---

## Privacy & security

Alethia is **local-first with zero telemetry by default.** Some of these guarantees are architectural (enforced in code, not policy); others are policy commitments that hold in v0.2 and any future opt-in cloud features will be clearly disclosed.

**Architectural guarantees** (enforced in code, can't drift):
- **Loopback only.** The MCP bridge only speaks to `127.0.0.1`. The desktop runtime's production webRequest filter blocks all non-`file://`, non-`app://`, non-`localhost` requests at the network layer.
- **Zero IPC.** No inter-process communication between driver and DOM — enforced by a CI gate (`scripts/check-zero-ipc.sh`) that fails the build if any forbidden API appears.
- **Sandboxed renderer.** The embedded browser runs in a locked-down sandbox with context isolation, no Node access, and full web security enabled.
- **Auditable bridge.** The MIT-licensed npm bridge is readable in minutes. It does nothing but forward MCP calls to localhost and (in v0.3) auto-download the signed runtime. Source: [github.com/vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp).

**Policy commitments** (true in v0.3; any future cloud features will be opt-in and clearly disclosed):
- **Zero telemetry collection by default.** The runtime does not phone home, does not collect usage metrics, does not report crashes anywhere out of the box.
- **Opt-in cloud features.** When the cloud dashboard / signed evidence service / agent observability layer ships, they will be explicit, separate, paid products you enroll in — not defaults that turn on silently.
- **Cryptographically signed evidence packs.** Ed25519 keypair + canonical SHA-256 manifest. See [Evidence](#evidence). Signing happens locally; any public key registry is opt-in.

---

## Evidence

The performance, safety, and patent claims are backed by reproducible evidence:

- **Benchmarks:** `click-assert-wait` against Playwright, Puppeteer, Selenium, Cypress, TestCafe, Taiko. Latest results show Alethia at 12.9 ms avg, Playwright at 580.7 ms avg.
- **Validation suite:** 12 jsdom-based unit tests + 13 integration tests against the live runtime covering navigation, assertions, typing, policy gate, integrity hashing, and the full dogfood flow.
- **Signed releases:** Ed25519-signed release manifests with per-artifact SHA-256 hashes. Verification happens automatically during auto-install.
- **Patent filing:** U.S. Patent Application No. 19/571,437 (non-provisional, in examiner queue), claiming priority to 63/785,814 filed April 9, 2025.

---

## Comparison to Playwright MCP

Microsoft ships [Playwright MCP](https://github.com/microsoft/playwright-mcp). Why use Alethia instead?

| | Playwright MCP | Alethia |
|---|---|---|
| Architecture | CDP marshalling, async-by-construction | Zero-IPC, synchronous |
| Avg latency per step | ~580 ms | **~13 ms** |
| Per-step safety gate | none | **VITRON-EA1 fail-closed** |
| Cryptographic audit | none | **SHA-256 chained, Ed25519 signable** |
| Telemetry | Microsoft data flow by default | **off by default; opt-in only** |
| Patent moat | none | **US 19/571,437** |
| Optimized for | cross-browser public-web QA | **agent loops on localhost dev servers** |

**Use Playwright MCP for** cross-browser public-web testing where the human is debugging async test scripts.

**Use Alethia for** AI agents driving localhost dev servers in tight decide-act-verify loops where every millisecond and every fail-closed guardrail matters.

The two tools serve different jobs. Playwright will not catch up on the agent-loop wedge because closing the gap would require abandoning their cross-browser story (they'd have to become an in-process, Chromium-only runtime — i.e. become Alethia).

---

## Patent Notice

Alethia practices a method that is the subject of:

- **U.S. Patent Application No. 19/571,437** (non-provisional)
- Claiming priority to **U.S. Provisional Application No. 63/785,814** (filed April 9, 2025)
- **Title:** *"Deterministic Local Automation Runtime with Zero-IPC Execution, Offline Operation, and Per-Step Policy Enforcement"*
- **Status:** Patent Pending — U.S. Patent and Trademark Office

The MIT license on the [alethia-mcp](https://github.com/vitron-ai/alethia-mcp) bridge does **not** grant any patent license under U.S. Application No. 19/571,437 or any other vitron.ai patent rights. The closed Alethia Core runtime is governed by separate terms; commercial and production use may require a patent license once granted.

For licensing inquiries: **gatekeeper@vitron.ai**

---

## Links

- 📦 **npm:** [npmjs.com/package/@vitronai/alethia](https://www.npmjs.com/package/@vitronai/alethia)
- 💾 **Bridge source (MIT):** [github.com/vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp)
- 📋 **Tessl Registry:** [tessl.io/registry/vitron-ai/alethia](https://tessl.io/registry/vitron-ai/alethia)
- 📦 **Releases:** [github.com/vitron-ai/alethia/releases](https://github.com/vitron-ai/alethia/releases)
- 🌐 **Website:** [vitron.ai](https://vitron.ai)
- 📧 **Licensing:** gatekeeper@vitron.ai
- 🏛 **Patent (USPTO):** Application No. 19/571,437

---

<div align="center">

**Alethia** is built by [vitron.ai](https://vitron.ai).
Patent Pending. Local-first. Built for the agents that build software.

</div>
