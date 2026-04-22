# Security

The canonical Alethia security posture — threat model, cryptographic chain of custody, local-only architecture, disclosure process — lives in the bridge repo:

**→ [vitron-ai/alethia-mcp/SECURITY.md](https://github.com/vitron-ai/alethia-mcp/blob/main/SECURITY.md)**

## Quick facts for reviewers

- **Local-only by architecture.** The runtime refuses to navigate to anything outside `file://`, `localhost`, `127.0.0.1`, `.local`, or RFC1918 ranges. The allowlist is a compile-time constant; no CLI flag, env var, or MCP argument expands it.
- **Ed25519-signed releases.** Every runtime release carries a signed manifest. The public key is embedded in the MCP bridge, verified end-to-end.
- **SHA-256 per-run integrity.** Every `alethia_tell` result includes a hash over the canonical PlanRun; evidence packs are hashed over the whole session.
- **No cloud telemetry.** Nothing leaves your machine unless you export an evidence pack yourself.
- **Destructive actions blocked unconditionally** under the VITRON-EA1 policy gate. Not a default, an invariant.

## Reporting security issues

**`gatekeeper@vitron.ai`** — do not include reproduction details in public GitHub issues. Expect acknowledgement within 72 hours.

## Reporting abuse

If you see Alethia being used against non-local origins via a modified binary or downstream fork: same contact, same SLA. We yank, stop signing, and pursue applicable legal channels.

## Patent

U.S. Application No. 19/571,437. The MIT license on the bridge does not grant a patent license to the runtime.
