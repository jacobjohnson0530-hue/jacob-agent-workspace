# Derivative verification status

This file records the bootstrap gates for Jacob Agent Workspace. It separates static identity checks from executable CI/runtime checks.

## Phase 1 — derivative bootstrap

Status: **complete**

Verified on `chore/derivative-bootstrap`:

- package name: `jacob-agent-workspace`
- package version: `2.2.0`
- Chrome companion version: `2.2.0`
- runtime `APP_VERSION`: `2.2.0`
- Electron app ID: `com.jacobagentworkspace.app`
- product name: `Jacob Agent Workspace`
- Linux executable name: `jacob-agent-workspace`
- Core MCP server: `jacob-agent-workspace-core`
- Desktop MCP server: `jacob-agent-workspace-desktop`
- Plugins MCP server: `jacob-agent-workspace-plugins`
- connector brand: `Jacob Agent Workspace`
- updater repository: `jacobjohnson0530-hue/jacob-agent-workspace`
- release artifact prefix: `Jacob-Agent-Workspace-`
- standalone extension recovery points at the derivative release repository
- renderer and extension product-facing strings use Jacob Agent Workspace branding
- release/publish workflows and packaging tests expect derivative artifact names
- upstream attribution and MIT license relationship are documented in `UPSTREAM.md`
- compatibility boundaries are documented in `IDENTITY-MIGRATION.md`

## Compatibility contract retained intentionally

The initial derivative does **not** rename the existing browser-bridge wire token `chat-on-steroids`.

Both the desktop bridge and the Chrome companion still use that exact token, preserving pairing compatibility while product-facing branding changes independently.

Existing `CLF_*`, `COS_*`, `cos.*`, continuation markers, persistent storage keys, and selected internal native identifiers are also retained unless an explicit migration is implemented.

## Phase 2 — executable verification

Status: **pending runtime execution**

Required gates:

1. `npm ci`
2. `npm run verify:ci`
3. `npm run build`
4. Windows packaged-app smoke test
5. Chrome extension load-unpacked smoke test
6. extension ↔ desktop bridge pairing
7. Core MCP initialize + tools/list + tool call
8. Desktop MCP initialize + browser/desktop smoke
9. Plugins MCP initialize + external-plugin discovery smoke
10. session / worker / Compact & Resume smoke test

The repository already contains a pull-request CI workflow that runs `npm run verify:ci` on Windows x64, macOS arm64, and Linux x64. At the time this file was written, the fork had produced no workflow run for PR #1, so executable verification remains a merge gate.

Do not publish a Jacob Agent Workspace release until the executable Phase 2 gates are green.
