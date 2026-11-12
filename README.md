# Xyvenra for Cursor

Cursor plugin for the production MCP at https://xyvenra.com/api/mcp. Connect with MCP OAuth 2.1 (GitHub or Google, PKCE). A human-minted platform API key is the fallback when that sign-in is unavailable. Plugin files live at the repository root.

This plugin was submitted to the Cursor Marketplace on November 11, 2026, and is pending Cursor's review. It is not listed yet.

## Local install

1. Copy this folder to `~/.cursor/plugins/local/xyvenra`, or symlink only if the target stays inside that local plugins directory.
2. Reload the window. When Cursor prompts, sign in with GitHub or Google. The shipped `mcp.json` is the remote URL only, with no `Authorization` header, so that sign-in can start.
3. Confirm the Xyvenra skill and MCP server are visible.
4. Ask Cursor to put a simple landing page online. It should create a Cloud site, deploy HTML, and return `https://xyvenra.com/sites/{slug}`.

## API key fallback

Use a platform API key only when OAuth cannot be completed. A human mints it at https://xyvenra.com/app/keys. The secret is shown once and starts with `xyv_plat_`. Do not put the key in this repo.

Cursor substitutes `${VAR}` headers on every request. This package does not declare `XYVENRA_API_KEY` or send `Authorization`, because a static Authorization header can block OAuth. There is no conditional-header field in the plugin format, and plugin-managed MCP config is read-only after install.

To use a key instead, add a user or project MCP entry and leave the plugin server disabled if both would connect:

```json
{
  "mcpServers": {
    "xyvenra": {
      "url": "https://xyvenra.com/api/mcp",
      "headers": {
        "Authorization": "Bearer xyv_plat_..."
      }
    }
  }
}
```

Replace the placeholder with the key the human minted. Never commit that file with a real key.

## Tools

The live server documents these tools: `create_project`, `list_projects`, `deploy`, `export`, `env_vars`, `database`, `files`, `eas_dispatch`, `guidance`, and `status`. Call a tool only when the connected server lists it, and follow that tool's schema.

`create_project` can create a Cloud site, a Data project, a Studio app, or a Mobile starter. `product: code` returns an error and points at https://xyvenra.com/app/code. `env_vars` can list, set, update, and delete values. Public values are written into the Cloud page at publish. Private values stay out of that page. Cloud has no server runtime.

## Policies

- Terms of use: https://xyvenra.com/terms
- Privacy notice: https://xyvenra.com/privacy

## Limits

Cloud serves self-contained static HTML (180 KB on the platform deploy path) or a static-site ZIP through a Cloud deploy key (4 MB). It is not a Node or Next.js host. Studio generation, Code editing, and Mobile builds stay in their own apps except for the MCP actions those tools expose: create, list, export, env vars, deploying a saved Studio page, and an explicit Expo workflow dispatch with the customer's own token. Paid Cloud, Data, Mobile, and Studio plans are not offered.
