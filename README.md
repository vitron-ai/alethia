<div align="center">

# Alethia

**The patent-pending zero-IPC E2E test runtime built for AI agents.**

**45× faster than Playwright** on the localhost test loop.
Fail-closed by default. Cryptographically chained audit packs.
**Local-first. Zero telemetry by default. Opt-in cloud.** No CDP.

[![npm](https://img.shields.io/npm/v/@vitronai/alethia.svg?label=%40vitronai%2Falethia&color=1fd67f)](https://www.npmjs.com/package/@vitronai/alethia)
[![License: MIT](https://img.shields.io/badge/MCP%20bridge-MIT-1fd67f.svg)](https://github.com/vitron-ai/alethia-mcp/blob/main/LICENSE)
[![Patent Pending](https://img.shields.io/badge/Patent-Pending-5fb4f7.svg)](#patent-notice)
[![Status](https://img.shields.io/badge/status-design%20partner%20alpha-e8a020.svg)](#status)

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

**Alethia is in design-partner alpha (v0.2.0) as of April 2026.**

- ✅ **MCP bridge published** to npm: [`@vitronai/alethia`](https://www.npmjs.com/package/@vitronai/alethia)
- ✅ **MCP bridge source** open and auditable: [vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp) — MIT
- ✅ **Patent filed**: U.S. Patent Application No. 19/571,437 (non-provisional, claiming priority to 63/785,814 filed April 9, 2025)
- ⏳ **Public binary releases** of the desktop runtime — coming with the design partner wave
- ⏳ **Headless mode** for unattended CI agents — v0.3 milestone
- ⏳ **Cloud dashboard** (signed evidence as a service, team collaboration) — post-MVP

**If you build AI coding agents and want early access to the desktop runtime**, email **gatekeeper@vitron.ai**. Design partner program is open and free during the alpha.

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
│  @vitronai/alethia     │  npm package, ~9 KB, MIT, source on github
│  stdio → HTTP shim     │  Zero telemetry. Loopback only (enforced).
└──────────┬─────────────┘
           │ HTTP POST 127.0.0.1:47432 (loopback only, never networked)
           ↓
┌────────────────────────┐
│  Alethia desktop app   │  Electron main process — proprietary, patent pending
│  local JSON-RPC server │  Loopback bind, never reachable from network
└──────────┬─────────────┘
           │ webContents.executeJavaScript('window.__alethia.tell(...)')
           ↓
┌────────────────────────┐
│  Alethia renderer      │  Electron renderer — IS the browser
│  zero-IPC runtime      │  tell() → NLP compiler → Action IR
└──────────┬─────────────┘  VITRON-EA1 policy gate (per-step, fail-closed)
           │ direct synchronous DOM access
           ↓
┌────────────────────────┐
│  The page under test   │  Your localhost dev server, your file://, your app://
└────────────────────────┘
```

**Two process boundaries** between your agent and the runtime (agent ↔ shim, shim ↔ Electron). Then **zero** boundaries between the runtime and the DOM. That's the architectural difference that makes Alethia 45× faster than Playwright on the localhost test loop.

---

## Try it

> ⚠️ **Design-partner alpha.** The desktop runtime is required and is currently distributed by request. Email **gatekeeper@vitron.ai** for early access. Public binary releases are coming with the v0.3 milestone.

### 1. Install the MCP bridge from npm (live now)

```bash
npm install -g @vitronai/alethia
```

### 2. Verify install

```bash
alethia-mcp --version
alethia-mcp --help
alethia-mcp --health-check
```

The health check will tell you the desktop runtime isn't reachable yet — that's expected if you don't have the binary. Once you do, it'll print:

```
✓ Connected. 5 MCP tools available.
  runtime version:  0.1.0-alpha.1
  default profile:  controlled-web
  kill switch:      inactive
```

### 3. Configure your agent

#### Claude Code (`~/.claude/mcp.json`)

```json
{
  "mcpServers": {
    "alethia": { "command": "alethia-mcp" }
  }
}
```

#### Cursor (Settings → MCP → Add server)

```json
{ "alethia": { "command": "alethia-mcp" } }
```

#### Cline / Continue / any MCP-compatible client

Same shape. They all speak the standard stdio MCP protocol.

### 4. Drive Alethia from your agent

> *"Use alethia_tell to navigate to localhost:3000, sign in as admin@example.com / password123, and verify the dashboard heading is visible."*

The agent will call `alethia_tell` with that NLP. Alethia compiles it to Action IR, runs it through the VITRON-EA1 policy gate (default: `controlled-web`, fail-closed), executes step-by-step with synchronous DOM access, and returns a signed `PlanRun` with per-step results, policy audit records, and a SHA-256 integrity hash.

---

## The 5 MCP tools

| Tool | Purpose |
|---|---|
| `alethia_tell` | Execute natural-language test instructions. The headline tool. ~13 ms per step. |
| `alethia_compile` | Compile NL → Action IR without executing. Preview before you run. |
| `alethia_status` | Health + identity probe. Version, profile, kill switch state, driver stats. |
| `alethia_activate_kill_switch` | Halt all automation immediately. Optional reason logged in audit trail. |
| `alethia_reset_kill_switch` | Clear an active kill switch. Re-enables `tell()` calls. |

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
- **Zero IPC.** No `ipcMain`/`ipcRenderer`/`contextBridge`/preload — enforced by a CI gate (`scripts/check-zero-ipc.sh`) that fails the build if any forbidden API appears.
- **Sandboxed renderer.** `sandbox: true`, `contextIsolation: true`, `nodeIntegration: false`, `webSecurity: true`.
- **Auditable bridge.** The MIT-licensed npm bridge is ~530 lines. Read it in 5 minutes and verify it does nothing but forward MCP calls to localhost. Source: [github.com/vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp).

**Policy commitments** (true in v0.2; any future cloud features will be opt-in and clearly disclosed):
- **Zero telemetry collection by default.** The runtime does not phone home, does not collect usage metrics, does not report crashes anywhere out of the box.
- **Opt-in cloud features.** When the cloud dashboard / signed evidence service / agent observability layer ships, they will be explicit, separate, paid products you enroll in — not defaults that turn on silently.
- **Cryptographically signed evidence packs.** Ed25519 keypair + canonical SHA-256 manifest. See [Evidence](#evidence). Signing happens locally; any public key registry is opt-in.

---

## Evidence

The performance, safety, and patent claims are backed by reproducible evidence:

- **Benchmarks:** `click-assert-wait` against Playwright, Puppeteer, Selenium, Cypress, TestCafe, Taiko. Latest results show Alethia at 12.9 ms avg, Playwright at 580.7 ms avg.
- **Validation suite:** 12 jsdom-based test cases (6 runtime + 6 SDK contract) covering parser, policy, executor, kill switch, integrity hashing.
- **Signed evidence packs:** Ed25519-signed canonical manifests via `npm run evidence:keygen` + `npm run evidence:pack` (in the closed alethia-core repo).
- **Patent filing:** U.S. Patent Application No. 19/571,437 (non-provisional, in examiner queue), claiming priority to 63/785,814 filed April 9, 2025.

Design partners get access to the full alethia-core repo (including benchmarks, evidence pipeline, and patent docs).

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

- 📦 **npm package:** [npmjs.com/package/@vitronai/alethia](https://www.npmjs.com/package/@vitronai/alethia)
- 💾 **MCP bridge source (MIT):** [github.com/vitron-ai/alethia-mcp](https://github.com/vitron-ai/alethia-mcp)
- 📧 **Design partner program / licensing:** gatekeeper@vitron.ai
- 🏛 **Patent (USPTO):** Application No. 19/571,437

---

<div align="center">

**Alethia** is built by [vitron.ai](https://vitron.ai).
Patent Pending. Local-first. Built for the agents that build software.

</div>
