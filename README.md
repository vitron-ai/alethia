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

- **Fast.** ~40 ms per tool call. 2–5× faster than Playwright MCP per flow; up to 50× faster than Playwright CLI on simple flows. [Reproduce the numbers yourself.](https://github.com/vitron-ai/alethia-anvil/blob/main/benchmark/README.md)
- **Safe.** Destructive actions (delete, purchase, transfer) are blocked by default. Your agent can't accidentally nuke prod data.
- **Private.** Runs entirely on your machine. No cloud. No telemetry. No data leaves localhost.

---

## Get started

### Claude Code users — one curl, Claude handles the rest

```bash
mkdir -p ~/.claude/skills/alethia && \
  curl -fsSL https://raw.githubusercontent.com/vitron-ai/alethia-mcp/main/skills/alethia/SKILL.md \
    -o ~/.claude/skills/alethia/SKILL.md
```

Restart Claude Code. Ask it to test a page or audit a site — Claude notices the tools aren't installed yet and walks you through the bridge install with verbatim commands. The skill bootstraps itself.

### Everyone else — two commands

**1. Install the bridge:**

```bash
npm install -g @vitronai/alethia
```

The runtime auto-downloads on first use. Signed and verified. No signup.

**2. Add to your MCP config** (Claude Desktop, Cursor, Cline, etc.):

```json
{
  "mcpServers": {
    "alethia": { "command": "alethia-mcp" }
  }
}
```

### Then go

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

| | Playwright CLI | Playwright MCP | Alethia |
|---|---|---|---|
| Who writes the tests | Humans, in JavaScript | An agent, via MCP | An agent, in plain English |
| Speed per call | ~2 s (process + browser respawn) | ~200 ms | **~40 ms** |
| Safety guardrails | None built in | None built in | **Fail-closed EA1 gate on destructive actions** |
| Signed evidence packs | No | No | **Yes** |
| Telemetry | On by default | Local-only | **None** |
| Optimized for | Human-written CI suites | General agent automation | **Agent-driven localhost verification** |

They solve different problems. [Full technical comparison](docs/TECHNICAL.md#comparison-to-playwright-mcp)

---

## Benchmark (reproducible)

Same agent, same app, same machine. GitHub Actions Ubuntu runner, N=10. All numbers reproducible via [alethia-anvil](https://github.com/vitron-ai/alethia-anvil/blob/main/benchmark/README.md).

### Per-flow timing

| Flow | What it tests | Alethia *(typical)* | PW MCP *(typical)* | PW CLI *(mean)* | vs PW MCP |
|---|---|---|---|---|---|
| smoke | Navigate + 4 text assertions | **42 ms** | 55 ms | 1.83 s | 1.3× |
| signin | Sign in, land on dashboard | **1.00 s** | 1.17 s | 2.76 s | 1.2× |
| crud | Add a task, verify it appears | **2.00 s** | 3.30 s | 3.20 s | 1.6× |
| search | Filter list, verify, clear | **996 ms** | 3.30 s | 2.88 s | **3.3×** |

### Per-flow token cost — Alethia wins every flow

What the agent reads (snapshot + step results) across one full flow. `cl100k_base` tokenizer.

| Flow | Alethia | PW MCP | |
|---|---|---|---|
| smoke | **242** | 287 | 1.2× fewer |
| signin | **304** | 810 | 2.7× fewer |
| crud | **376** | 3,175 | **8.5× fewer** |
| search | **538** | 3,617 | **6.7× fewer** |

Alethia returns one compact response per flow. PW MCP returns an accessibility snapshot after every action — these sum. On a simple demo page the gap is modest; **on production apps with complex accessibility trees, the ratio widens.**

### Suite total (N=10, incl. install cost)

| | Alethia | PW CLI | PW MCP |
|---|---|---|---|
| Install (one-time) | 369 ms | 10.11 s | 2.07 s |
| 4 flows × 10 runs | 50.53 s | 106.68 s | 84.62 s |
| **Total** | **50.90 s** | 116.79 s | 86.69 s |

**Alethia is 2.3× faster than Playwright CLI and 1.7× faster than Playwright MCP end-to-end** on a realistic four-flow agent session.

The three-target harness, its caveats, and the install-cost methodology all live in the [reproducibility kit](https://github.com/vitron-ai/alethia-anvil/blob/main/benchmark/README.md). Clone it, run `npm install`, and generate the same numbers yourself.

---

## Who it's for

- **Agent tool builders** — give your agents eyes and hands in the browser
- **AI coding assistants** — verify generated code actually works
- **Enterprise & regulated teams** — air-gapped, auditable, no data exfiltration by default

---

## Go deeper

- [Agent cookbook](https://github.com/vitron-ai/alethia-mcp/blob/main/docs/agent-cookbook.md) — paste-ready prompts for full demos (smoke tests, compliance audits, parallel checks, EA1 safety proofs, partner walkthroughs)
- [UI for agents](https://github.com/vitron-ai/alethia-mcp/blob/main/docs/ui-for-agents.md) — designing UIs to be driven by AI: the resolver priority, when to add `data-alethia` hooks, anti-patterns from Playwright/Cypress that don't apply
- [Security posture](https://github.com/vitron-ai/alethia-mcp/blob/main/SECURITY.md) — threat model, cryptographic chain of custody, supply-chain posture, update cadence, disclosure process
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
