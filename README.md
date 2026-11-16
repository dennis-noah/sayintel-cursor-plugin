# SayIntel plugin

Agent Plugin that connects Cursor to SayIntel's hosted MCP server. It turns a conference speaker page into a researched outbound campaign.

This package has no secrets. Cursor handles sign-in with OAuth (dynamic client registration and a consent screen) when you connect.

## What's included

- `plugin.json` — Agent Plugins manifest
- `mcp.json` — remote Streamable HTTP MCP at `https://sayintel.com/mcp` (100 read and write tools; no send tools)
- `skills/sayintel/SKILL.md` — when and how an agent should use the tools
- `LISTING.md` — draft Cursor Marketplace copy (not submitted)

## Use it in Cursor

1. Copy this folder to `~/.cursor/plugins/local/sayintel` and run **Developer: Reload Window**.
2. Sign in to SayIntel when Cursor opens the consent screen.
3. Ask it to start a campaign from a conference URL, check a run, or review approved leads.

Pushing to sending tools, bulk approve-and-push, inviting teammates and setting integration keys are not available through this connector. `approve_message` can mark a single draft approved but never sends, and it is refused on campaigns with auto-push armed.

## Troubleshooting

If you disconnect SayIntel in SayIntel Settings, the connection stays revoked. Reconnect from Cursor to consent again.

## Docs

- https://sayintel.com/docs
- https://sayintel.com/docs/tools
- https://sayintel.com/docs/authentication
- https://sayintel.com/docs/api

Support: dennis@theonemile.com
