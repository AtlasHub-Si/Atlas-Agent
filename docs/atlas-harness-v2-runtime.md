# Atlas Harness V2 — Hermes runtime integration contract (draft)

Date: 2026-11-12
Branch: feat/atlas-harness-v2-aws
Status: specification only; no production installation or runtime behavior changed.

## Goal

Make Atlas-Agent/Hermes the default agent runtime of a portable Atlas Harness edition that can operate on a customer's PC, customer's VPS, or Atlas Cloud. The same Atlas Desktop client should select and securely switch across separate gateways without mixing their state.

## Existing source capabilities and constraints

This repository already includes Hermes Desktop at `apps/desktop` (Electron/React), Hermes headless `serve` backend over JSON-RPC/WebSocket, documented local and remote connection modes, installers/build scripts and platform update flows. This is SOURCE, not an Atlas-branded signed release. No AtlasHub-Si releases found on the audit date.

The OpenBot fork in AtlasHub-Si/Atlas-Bot includes a Tauri Desktop app, governance and browser-computer components. Its present personal edition is principally a local stack installer with CopilotKit Intelligence licensing. Neither app should be represented as a finished Atlas multi-instance customer product.

## Runtime contract

- `installation_id`: immutable instance identifier independent from device.
- `tenant_id`: authenticated organization scope for all missions, knowledge and tools.
- `connection_profile_id`: client-side saved target; local / own-vps / atlas-cloud.
- `runtime_kind`: hermes; optional adapters must identify their backend explicitly.
- `mission_id`, `correlation_id`, `approval_policy`, `allowed_tools`, `budget_limit`, `deadline`.
- Events: requested, authorized, queued, running, approval_required, completed, failed, cancelled.
- Every privileged external operation goes via Atlas Control API / guarded tool proxy.
- A subagent cannot widen parent data scope, tool access, or budget.
- Do not move saved provider keys or private memory to a remote gateway implicitly.

## Minimum test matrix

1. Windows PC starts local Hermes with Atlas free-profile and Creator Pack.
2. Linux VPS runs Hermes as a service with TLS, restricted ingress, valid auth and durable volumes; the PC connects via the same Desktop.
3. A second remote profile can be added and switched to without displaying previous gateway's sessions.
4. Remote schedules continue when PC app exits.
5. Docker sandbox denies cross-agent filesystem and network escape; no host Docker socket to untrusted tools.
6. Tool approvals and cancellations roundtrip from Desktop through Atlas Control API to Hermes.
7. A failed install can be rolled back; restore of backup and idempotent upgrades are verified.

## Product integration steps

1. Pin Hermes source version/commit. Keep upstream MIT notices and update security review.
2. Define local/remote capabilities discovery, auth, session binding and reconnect semantics.
3. Build the Atlas-brand integration as adapter and UI skin without a permanent direct fork of unrelated upstream layers.
4. Implement install manifests and Agency Pack contract in Atlas-Harness canonical repository once created in AtlasHub-Si.
5. Publish our own signed/verified artifacts from AtlasHub-Si CI and GHCR; do not repoint OpenBot/Hermes official download URLs.
6. Do not deploy production services or publish customer downloads before integration, rights and QA gates.

Related: https://github.com/AtlasHub-Si/Atlas-Bot/blob/feat/atlas-harness-v2-aws/docs/atlas-harness-v2-aws-plan.md
