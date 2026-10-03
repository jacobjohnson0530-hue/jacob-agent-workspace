# Release hardening — 2026-10-04

This worklog closes the runtime gates that remained after the Goal/Loop helper-readiness fix. All live file operations stayed inside the approved disposable test workspace. PR #2 remained Draft and was not merged.

## Runtime gates

- **Compact & Resume:** the first post-resume calls correctly failed closed until the desktop Chrome conversation re-established exact caller identity. No unattributed-call bypass was enabled. After identity recovery, Core read the existing smoke file successfully and no completed Worker, Goal, or Loop action was replayed.
- **Desktop:** a fresh safe-scope smoke covered window state, browser state, navigation, click, text input, keyboard input, and scrolling. Temporary smoke fixtures were created only in the disposable test root, then removed. The existing smoke file remained unchanged.
- **Loop `afterTurn: false`:** a fresh Loop chat delivered multiple helper-generated continuations automatically into the same executor chat. Each continuation caused a new Core read of the same existing smoke file. Loop was paused immediately after the evidence was collected.
- **Plugins surface:** a dedicated Plugins tunnel was created and connected as `Jacob Agent Workspace Plugins` through ChatGPT's custom MCP flow with no authentication. ChatGPT reports the connector as connected. JAW reports **0 tools available**, which is the expected state because no external MCP integration is enabled. Core, Desktop, and Plugins remain separate surfaces.

## Current-head verification

Head before this documentation update: `efc719dc8dc3e3e7f576b3e0192c8a85a244d78f`.

- GitHub Actions CI run `37124153683`: Windows x64, macOS arm64, and Linux x64 all passed.
- Local broad `npm run verify`: privacy, notices, typecheck, Electron load, and the main Vitest phase ran. The main phase recorded **6,340 passed, 48 skipped, 3 failed** under full parallel load:
  - `test/mcp.test.ts`: exec-session owner recovery did not observe its delayed output before the assertion.
  - `test/renderer-state.test.ts`: a delayed provider-state save had not reached the third recorded call before the assertion.
  - `test/exec-hints.test.ts`: the real PowerShell cut-pipeline probe exceeded the 30-second test timeout.
- Each exact failed case passed immediately when rerun alone.
- The three complete affected files were then rerun with `--maxWorkers=1`: **355 passed, 6 skipped**.
- The `verify:ci` serialized tail that broad verification had not reached because of the main-phase exit was run separately: `test/computer.test.ts` + `test/mcp-shutdown.test.ts` passed **26/26**.

The evidence supports classifying the three broad-run failures as local parallel-load/timing flakes rather than a product regression. This does not relabel the broad run itself as green.

## Windows x64 package

A fresh `npm run dist:x64` completed successfully on the current source and ran its packaged-runtime smoke automatically.

- packaged version: `2.2.0`
- Electron: `44.3.0`
- MCP runtime load: passed
- Sharp/libvips probe: passed
- node-pty probe: passed
- tree-sitter probe: passed
- Windows pet-focus native probe: passed
- installer size: `170315296` bytes
- installer SHA-256: `D541F64517ADD62F70D00637FE99FCE605B4E114F0E96EF95758B2AD359794A8`
- source `extension/content.js` SHA-256: `CBFB429D7E84C49612F731249F9F4E9E9047AF49D1F660DAE9ED04E515370432`
- packaged `extension/content.js` SHA-256: `CBFB429D7E84C49612F731249F9F4E9E9047AF49D1F660DAE9ED04E515370432`

The package build is unsigned; electron-builder explicitly reported that no code-signing certificate is configured. The packaged-runtime smoke emitted a non-fatal Crashpad named-pipe diagnostic after the successful probe.

## Remaining release gate

The exact current-head NSIS installer has not been installed over the maintainer's normal per-user installation in this safe-scope session. Installing it would intentionally modify state outside the disposable test root (application install state, shortcuts and uninstall metadata), so that destructive boundary remains a separate explicit release action.

Until that install/readback is deliberately completed:

- keep PR #2 Draft;
- do not merge PR #2;
- do not publish a public 2.2.0 release.
