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

5. ✅ Windows x64 package generation + packaged-runtime smoke (Actions run `36473828817`, plus a fresh current-head local rebuild)
6. ⚠️ Installed 2.2.0 application launched and served the test chats; exact current-head NSIS installer install/readback remains open
7. ✅ Chrome extension load-unpacked, current helper-readiness deployment, reload, and live recheck
8. ✅ extension ↔ desktop bridge pairing in Worker, Goal/Loop, Compact/Resume, and final runtime verification
9. ✅ Core MCP tool calls in the disposable test workspace, including fresh reads after Compact & Resume and automatic Loop continuation
10. ✅ Desktop MCP safe-scope smoke after identity recovery: window state, browser state, navigation, click, typing, keyboard, and scroll
11. ✅ Plugins MCP surface initialization: a dedicated Plugins tunnel is connected in ChatGPT and truthfully publishes an empty tool catalog when no external MCP integrations are enabled
12. ✅ session, Worker lifecycle, three-Worker routing, Compact & Resume, explicit Goal/Loop, and automatic Loop `afterTurn: false` live smoke

The helper editor-readiness regression is now live-verified. Source commit `c017096` passed three-platform CI run `37123484602`; documentation head `efc719d` passed three-platform CI run `37124153683`. The only remaining install-specific release gate is an exact current-head NSIS install/readback. Keep the PR Draft and do not publish a public release until that gate is intentionally exercised.

## Phase 2C — live runtime and current local verification

The dated evidence and limits are in `docs/worklog-2026-10-03-goal-helper-remount.md`. The safe workspace is `C:\Users\jacob\Desktop\jacob-agent-test`; smoke reads used its existing `test.txt` and did not modify sensitive project data.

- The Worker bootstrap, finish/sleep/wake lifecycle, and three concurrent Worker reports passed. Core and Desktop use separate tunnel surfaces.
- Explicit Goal and Loop helpers each delivered a follow-up into the owning ChatGPT conversation and caused a second Core read. The editor-remount case is covered by positive and foreign-draft regression tests.
- Compact & Resume moved chat A to B with the same handoff marker; B performed a new Core read. No completed Worker, Goal, or Loop action was replayed.
- Automatic Loop continuation initially failed before insertion with `content_delivery_editor_not_writable`. The current source rechecks layout readiness within the existing 15-second ownership-bound wait. Its positive and permanent-failure regression cases passed, and the updated companion subsequently completed the automatic `afterTurn: false` live flow with repeated fresh Core reads before Loop was paused.
- On 2026-10-03, local `npm run verify` passed privacy, notices, and typecheck, then recorded 6,334 passed and 10 failed tests. Eight failures were generated PowerShell scripts rejected by the local execution policy. Two timing-sensitive cases passed on isolated rerun. The test launchers now use a process-scoped policy override, consistent with the existing Windows capture verification script; six affected test files passed 132/132 on targeted rerun. GitHub Actions run `37123484602` then passed the full Windows x64, macOS arm64, and Linux x64 jobs on source commit `c017096`.

## Phase 2D — final runtime and package hardening

The final 2026-10-04 evidence is recorded in `docs/worklog-2026-10-04-release-hardening.md`.

- Compact/Resume caller identity recovered without enabling unattributed calls; the resumed conversation completed a new Core read and did not replay completed work.
- A fresh Desktop smoke used only disposable fixtures in the approved test root and removed them afterward.
- Automatic Loop `afterTurn: false` delivered multiple helper continuations into the same executor chat; each continuation triggered a fresh Core read, then automation was paused.
- The separately tokenized Plugins connector is installed and connected in ChatGPT. JAW reports zero published plugin tools because no external MCP integrations are currently enabled; that empty catalog is the expected fresh state.
- A current-head Windows x64 rebuild passed packaged native-runtime smoke. The rebuilt installer is `170315296` bytes with SHA-256 `D541F64517ADD62F70D00637FE99FCE605B4E114F0E96EF95758B2AD359794A8`; packaged and source `extension/content.js` both hash to `CBFB429D7E84C49612F731249F9F4E9E9047AF49D1F660DAE9ED04E515370432`.
- A local broad verification run recorded **6,340 passed, 48 skipped, 3 failed** under full parallel load. The three failures were timing-sensitive session, renderer-state, and PowerShell pipeline cases. Each exact case passed immediately in isolation, and the three complete affected files then passed serially with **355 passed, 6 skipped**. The separately serialized `computer.test.ts` + `mcp-shutdown.test.ts` phase passed **26/26**. Current-head three-platform CI remains green.

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
