<div align="center">

# Alethia

**Your AI agent tests your app with plain English.**

One command. No test scripts. No browser drivers. Just tell it what to check.

[![npm](https://img.shields.io/npm/v/@vitronai/alethia.svg?label=%40vitronai%2Falethia&color=1fd67f&logo=npm&logoColor=white)](https://www.npmjs.com/package/@vitronai/alethia)
[![License: MIT](https://img.shields.io/badge/MCP%20bridge-MIT-1fd67f.svg?logo=opensourceinitiative&logoColor=white)](https://github.com/vitron-ai/alethia-mcp/blob/main/LICENSE)
[![Patent Pending](https://img.shields.io/badge/Patent-Pending-5fb4f7.svg?logo=shield&logoColor=white)](#patent-notice)

</div>

---

## What it does

You tell your AI agent:

> *"Navigate to my app, type test@example.com into the email field, click Sign In, and assert the dashboard is visible."*

Alethia makes it happen — in a real browser, on your machine, in milliseconds. No Selenium. No Playwright scripts. No flaky async waits.

**Built for AI agents**, not humans writing test code.

---

## Three things that matter

- **Fast.** ~13 ms per step. 45x faster than Playwright. Your agent can test as fast as it can think.
- **Safe.** Destructive actions (delete, purchase, transfer) are blocked by default. Your agent can't accidentally nuke prod data.
- **Private.** Runs entirely on your machine. No cloud. No telemetry. No data leaves localhost.

---

## Get started

### 1. Install

```bash
npm install -g @vitronai/alethia
```

The runtime auto-downloads on first use. Signed and verified. No signup.

### 2. Add to your AI agent

Add to your MCP config (Claude Code, Cursor, Cline, etc.):

```json
{
  "mcpServers": {
    "alethia": { "command": "alethia-mcp" }
  }
}
```

### 3. Go

Ask your agent to test something. It calls `alethia_tell` under the hood:

```
"Navigate to http://localhost:3000, click Sign In, assert the dashboard is visible."
```

Each step returns pass/fail results, a snapshot of what the page looks like, and a signed audit trail.

---

## What your agent gets back

Every `alethia_tell` call returns:

- **Per-step results** — pass/fail with detailed context on failures
- **Page snapshot** — ~200 tokens describing what's on screen (not a raw DOM dump)
- **Safety audit** — every step checked against the VITRON-EA1 policy gate
- **Integrity hash** — SHA-256 chained proof of what happened and when

---

## Tools available to your agent

| Tool | What it does |
|---|---|
| `alethia_tell` | Run plain-English test steps. The main tool. |
| `alethia_tell_parallel` | Run multiple test flows concurrently. |
| `alethia_screenshot` | Capture a PNG of the current page. |
| `alethia_compile` | Preview what `tell` will run, without executing. |
| `alethia_eval` | Run raw JavaScript in the page. Escape hatch. |
| `alethia_status` | Health check — version, config, kill switch state. |
| `alethia_audit_wcag` | WCAG 2.1 AA accessibility audit. |
| `alethia_audit_nist` | NIST SP 800-53 security controls audit. |
| `alethia_export_session` | Export a signed evidence pack of the session. |
| `alethia_activate_kill_switch` | Emergency halt. Stops all automation immediately. |
| `alethia_reset_kill_switch` | Resume after a kill switch activation. |

---

## Try the built-in demo

Alethia ships with demo apps you can drive immediately. Paste this into Claude Code:

```
Use alethia_serve_demo to start the demo server. Then use
alethia_tell to navigate to the claude-code-app URL. Assert
"TaskFlow" is visible. Type dev@company.com into the
"you@company.com" field. Type Engineering into the "Your team
name" field. Click Sign in. Assert "Signed in as" is visible.
Type "Deploy to production" into the "Add a new task" field.
Click Add. Assert "Deploy to production" is visible. Click
Delete and report what EA1 decides.
```

The agent starts a localhost server, drives the app with plain English, and EA1 blocks the delete.

**Watch it live:** The Alethia cockpit opens by default — you'll see each step highlighted on-screen (green = pass, blue = type, red = EA1 block). Set `ALETHIA_HEADLESS=1` to run without a visible window (recommended for CI; auto-detected in common CI environments).

---

## How is this different from Playwright?

Playwright is built for humans writing test scripts against public websites. Alethia is built for AI agents verifying apps on localhost.

| | Playwright | Alethia |
|---|---|---|
| Who writes the tests | Humans, in JavaScript | AI agents, in plain English |
| Speed per step | ~580 ms | **~13 ms** |
| Safety guardrails | None built in | **Blocked by default for destructive actions** |
| Telemetry | On by default | **None** |
| Optimized for | Cross-browser QA | **Agent-driven localhost verification** |

They solve different problems. [Full technical comparison](docs/TECHNICAL.md#comparison-to-playwright-mcp)

---

## Who it's for

- **Agent tool builders** — give your agents eyes and hands in the browser
- **AI coding assistants** — verify generated code actually works
- **Enterprise & regulated teams** — air-gapped, auditable, no data exfiltration by default

---

## Go deeper

- [Technical architecture & benchmarks](docs/TECHNICAL.md) — zero-IPC design, EA1 policy spec, evidence packs, patent details
- [MCP bridge source (MIT)](https://github.com/vitron-ai/alethia-mcp) — auditable in minutes
- [npm package](https://www.npmjs.com/package/@vitronai/alethia)
- [Releases](https://github.com/vitron-ai/alethia/releases)

---

## Patent Notice

Alethia is patent pending (U.S. Application No. 19/571,437). The MIT license on the [MCP bridge](https://github.com/vitron-ai/alethia-mcp) does **not** grant a patent license for the Alethia runtime. Commercial use may require a separate license.

For licensing & partnerships: **gatekeeper@vitron.ai**

---

<div align="center">

**Alethia** is built by [vitron.ai](https://vitron.ai).
Patent Pending. Local-first. Built for the agents that build software.

</div>
