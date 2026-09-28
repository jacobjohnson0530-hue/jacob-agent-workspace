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

Status: **CI verification passed on Windows x64, macOS arm64, and Linux x64**

GitHub Actions run `36472439512` passed on all three supported CI targets. Each target completed:

- `npm ci`
- `npm run verify:ci`
- `npm run build`
- Python 3.12 setup
- `uv==0.12.5` plugin runtime setup
- live plugin proxy/Python/catalog integration tests

This establishes cross-platform source/build/plugin verification for the derivative bootstrap.

Completed CI gates:

1. ✅ `npm ci`
2. ✅ `npm run verify:ci`
3. ✅ `npm run build`
4. ✅ live external-plugin integration tests on Windows/macOS/Linux

Remaining local/runtime gates before a public release:

5. ⬜ Windows packaged-app install/start smoke test
6. ⬜ Chrome extension load-unpacked smoke test
7. ⬜ extension ↔ desktop bridge pairing
8. ⬜ Core MCP initialize + tools/list + tool call
9. ⬜ Desktop MCP initialize + browser/desktop smoke
10. ⬜ Plugins MCP initialize + external-plugin discovery smoke
11. ⬜ session / worker / Compact & Resume smoke test

The derivative bootstrap is now source/build CI-clean. Do not publish a public Jacob Agent Workspace release until the remaining local/runtime gates are exercised.

<!-- CI trigger marker: Actions enabled for derivative verification -->


## First executable CI findings

The first three-platform run validated dependency installation on Windows, macOS, and Linux, then failed in `npm run verify:ci` on a common set of derivative-rebranding assertions rather than OS-specific runtime behavior.

The follow-up fixes align:
- model-facing instruction expectations
- Goal transcript branding
- plugin UI assertions
- pet and skill error assertions
- renderer source/i18n keys
- Jacob Agent Workspace 2.2.0 changelog and release notes

This branch exists to rerun the executable gate against the corrected head.
