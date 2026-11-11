# Xyvenra for Cursor

Cursor plugin that wraps the production MCP at https://xyvenra.com/api/mcp with a bearer platform API key, plus a skill for the publish flow. Plugin files live at the repository root.

This repository is not listed in the Cursor Marketplace. A future submission, if approved, would be made at https://cursor.com/marketplace/publish. It has not been submitted yet.

## Local install

1. Copy this folder to `~/.cursor/plugins/local/xyvenra`, or symlink only if the target stays inside that local plugins directory.
2. Mint a key at https://xyvenra.com/app/keys and set `XYVENRA_API_KEY` in Cursor under Plugins, Configure. Do not put the key in this repo.
3. Reload the window and confirm the Xyvenra skill and MCP server are visible.
4. Ask Cursor to put a simple landing page online. It should create a site, deploy HTML, and return `https://xyvenra.com/sites/{slug}`.

## Policies

- Terms of use: https://xyvenra.com/terms
- Privacy notice: https://xyvenra.com/privacy

## Limits

Xyvenra Cloud serves self-contained static HTML only. It is not a Node or Next.js host. Accounts can have at most three Cloud sites. The `env_vars` tool is not available yet.
