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

5. ✅ Windows x64 package generation + packaged-runtime smoke (Actions run `36473828817`, artifact `jacob-agent-workspace-windows-x64-smoke`)
6. ⚠️ Installed 2.2.0 application launched and served the test chats; exact current-head installer install/readback remains open
7. ⚠️ Chrome extension load-unpacked and previous reload passed; the latest editor-readiness patch still needs a Chrome reload and live recheck
8. ✅ extension ↔ desktop bridge pairing in the earlier Worker and Goal/Loop live runs
9. ✅ Core MCP tool calls in the safe test workspace, including a new read after Compact & Resume
10. ⚠️ Desktop MCP browser/desktop actions were reported in the resumed chat; repeat the full safe-scope smoke after the current connection and identity recover
11. ⬜ Plugins MCP initialize + external-plugin discovery smoke
12. ✅ session, Worker lifecycle, three-Worker routing, and Compact & Resume smoke; explicit Goal/Loop helper delivery also passed

The automatic Loop continuation (`afterTurn: false`) failed once at editor readiness. A focused source and regression-test fix is present, but the updated companion has not yet been reloaded and rechecked live. Source commit `c017096` passed the three-platform CI run `37123484602`. Do not publish a public Jacob Agent Workspace release until the remaining local/runtime gates are exercised against the current head.

## Phase 2C — live runtime and current local verification

The dated evidence and limits are in `docs/worklog-2026-10-03-goal-helper-remount.md`. The safe workspace is `C:\Users\jacob\Desktop\jacob-agent-test`; smoke reads used its existing `test.txt` and did not modify sensitive project data.

- The Worker bootstrap, finish/sleep/wake lifecycle, and three concurrent Worker reports passed. Core and Desktop use separate tunnel surfaces.
- Explicit Goal and Loop helpers each delivered a follow-up into the owning ChatGPT conversation and caused a second Core read. The editor-remount case is covered by positive and foreign-draft regression tests.
- Compact & Resume moved chat A to B with the same handoff marker; B performed a new Core read. No completed Worker, Goal, or Loop action was replayed.
- Automatic Loop continuation later failed before insertion with `content_delivery_editor_not_writable`. The current source rechecks layout readiness within the existing 15-second ownership-bound wait. Its positive and permanent-failure regression cases passed; a fresh live pass is still required.
- On 2026-10-03, local `npm run verify` passed privacy, notices, and typecheck, then recorded 6,334 passed and 10 failed tests. Eight failures were generated PowerShell scripts rejected by the local execution policy. Two timing-sensitive cases passed on isolated rerun. The test launchers now use a process-scoped policy override, consistent with the existing Windows capture verification script; six affected test files passed 132/132 on targeted rerun. GitHub Actions run `37123484602` then passed the full Windows x64, macOS arm64, and Linux x64 jobs on source commit `c017096`.

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


## Windows package artifact

Phase 2B one-shot package smoke completed successfully in GitHub Actions run `36473828817`.

Artifact:
- name: `jacob-agent-workspace-windows-x64-smoke`
- size: `170324813` bytes
- SHA-256 artifact digest: `a6e5a8af8a65f7ee11da3cd4c7b1a45b42b981143061bc1b58d0d61693f73247`
- retention: through 2026-10-12
- source head: `daff9301456e877a791a3370b176affe75ddde22`

The Windows x64 packaging job passed `npm run dist:x64`, `smoke-packaged-runtime.mjs`, installer existence/size validation, and artifact upload.
