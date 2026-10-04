# Derivative identity and compatibility ledger

Jacob Agent Workspace is derived from `totec448-spec/chat-on-steroids`. Product-facing identity is being separated from upstream while selected internal identifiers are intentionally retained for compatibility.

## Product identity

New derivative identity:

- Product: **Jacob Agent Workspace**
- Package: `jacob-agent-workspace`
- Repository: `jacobjohnson0530-hue/jacob-agent-workspace`
- Electron app ID: `com.jacobagentworkspace.app`
- Core MCP server: `jacob-agent-workspace-core`
- Desktop MCP server: `jacob-agent-workspace-desktop`
- Plugins MCP server: `jacob-agent-workspace-plugins`
- Release artifacts: `Jacob-Agent-Workspace-*`

## Compatibility identifiers intentionally retained

Do not rename these by search-and-replace. They are compatibility or migration boundaries and require an explicit protocol/storage migration if changed.

### Browser bridge wire identity

`chat-on-steroids`

The Chrome companion currently verifies the app using this wire token. Renaming it without a coordinated bridge protocol migration would make the extension treat the local app as an unknown peer.

### CLF protocol/runtime namespace

Examples:

- `CLF_DOM`
- `CLF_I18N`
- `CLF_BRIDGE_PORTS`
- `CLF_EVIDENCE_MS`
- `[[CLF-HANDOFF:...]]`
- `[[CLF-RESUME:...]]`
- `clf` / `clf_project` URL markers

These names participate in browser-extension, test, environment, continuation, or DOM contracts. They remain stable in the initial derivative.

### COS environment and content markers

Examples include `COS_PLUGIN_LIVE_TEST`, `COS_PACKAGE_ARCH`, `COS_CONTEXT`, and `COS_GOAL`.

These are retained initially because CI, packaging, tests, and existing serialized content may depend on them.

### Existing storage keys and probes

Retained initially:

- `.chat-on-steroids-source`
- `chat-on-steroids.sidebar-completion-seen`
- `chat-on-steroids.sidebar-order`
- `chat-on-steroids.sidebar-width`
- `chat-on-steroids.bottom-panel-height`
- `chat-on-steroids-safe-storage-probe`

Changing persistent keys requires a read-old/write-new migration so existing settings/history are not silently lost.

## Historical material

Old release notes, worklogs, contributor records, upstream issue/PR links, protocol-history comments, and attribution may continue to say **Chat On Steroids**. Those are historical references, not current product identity.

## Migration rule

A compatibility identifier may be renamed only when all readers and writers are migrated together, tests cover old-to-new upgrade behavior, and rollback/recovery behavior is defined. Until then, product branding and protocol identity are intentionally separate.
