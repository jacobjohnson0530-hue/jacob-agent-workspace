# Upstream relationship

Jacob Agent Workspace is a derivative work of [Chat On Steroids](https://github.com/totec448-spec/chat-on-steroids).

## Repositories

- Upstream: `totec448-spec/chat-on-steroids`
- Derivative: `jacobjohnson0530-hue/jacob-agent-workspace`
- Upstream branch: `main`
- Derivative integration branch: `main`
- Initial customization branch: `chore/derivative-bootstrap`

## License and attribution

The upstream project is distributed under the MIT License. The original MIT license and copyright notice remain in this repository and must remain in substantial copies or distributions of upstream-derived code.

Local branding, features, fixes, and derivative changes do not remove the upstream attribution requirements.

## Upstream synchronization policy

Upstream changes should be reviewed through a dedicated branch or pull request rather than merged blindly. Pay particular attention to changes involving:

- MCP surfaces and tool schemas
- the Chrome companion and ChatGPT DOM integration
- command execution and terminal permissions
- filesystem sandbox boundaries
- worker, session, Goal/Loop, and compaction durability
- browser bridge pairing and authentication
- secrets and tunnel configuration
- packaging, signing, releases, and auto-update behavior

The goal is to preserve a clear audit trail between upstream behavior and Jacob Agent Workspace-specific changes.
