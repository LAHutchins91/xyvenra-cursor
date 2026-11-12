---
name: publish-with-xyvenra
description: Publish a static site on Xyvenra Cloud, or use the official Xyvenra MCP for Data, Studio, Mobile, and env vars. Use when the user wants a landing page or HTML site online, a Xyvenra Cloud deploy, project env vars, a Data project, a Studio app, or a Mobile starter.
---

# Publish with Xyvenra

Use the Xyvenra MCP at https://xyvenra.com/api/mcp. Official registry name: `io.github.LAHutchins91/xyvenra`. Docs: https://xyvenra.com/docs/api.

Connect with MCP OAuth 2.1 first. Production publishes protected-resource metadata at https://xyvenra.com/.well-known/oauth-protected-resource and authorization-server metadata at https://xyvenra.com/.well-known/oauth-authorization-server. Sign-in reuses GitHub or Google with PKCE. This plugin's `mcp.json` is the remote URL only, so Cursor can start that sign-in.

If OAuth cannot be completed, a human mints a platform API key at https://xyvenra.com/app/keys. It is shown once and starts with `xyv_plat_`. Send it only as `Authorization: Bearer`. Agents cannot complete signup or mint the key. Never invent a key, paste one into the conversation, or write one into a committed file.

After the server is connected, call only tools it lists and follow each tool's input schema. If this skill and the live schema disagree, follow the schema.

## When not to use it

- Cloud serves self-contained static HTML (180 KB on this deploy path). A static-site ZIP uses a Cloud deploy key and a 4 MB cap. Cloud is not a Node, Next.js, or general app host, and it has no server runtime.
- `create_project` with `product: code` returns an error and points at https://xyvenra.com/app/code. Code chat and file editing stay in the app.
- Studio generation, visual editing, and hosted builds stay in the app. This MCP can create a Studio app, list it, export its source ZIP, and deploy a saved page.
- Mobile in-app preview and store submission stay outside this MCP. This MCP can create a starter, list it, store env vars, export the Expo ZIP, and dispatch one existing Expo workflow when the human supplies their own token. Creating or exporting a starter does not start a build. Xyvenra does not pay Expo.
- Paid Cloud, Data, Mobile, and Studio plans are not offered. Do not claim a checkout or a charge for those products.

## Cloud publish

1. Confirm the MCP server is connected through OAuth, or through a human-minted platform key when OAuth is unavailable.
2. Call `create_project` with `product` `cloud`, a `name`, and a `slug`. Keep the returned site id. Use the tool schema for the slug rules.
3. Call `deploy` with `siteId` and self-contained `html`. Public env vars for that site are written into the page. Private values are not.
4. Give the human the live URL from the tool result. It is `https://xyvenra.com/sites/{slug}`.

## Tools

The API reference documents these tools. Use one only when `tools/list` includes it.

- `create_project` creates a Cloud site (`name` and `slug`), a Data project (`name`), a Studio browser app, or a Mobile Expo starter. It does not create a Code project.
- `list_projects` lists owned rows for one of `cloud`, `data`, `studio`, `mobile`, or `code`.
- `deploy` publishes self-contained HTML to a Cloud site this account owns. Pass `projectId` instead of `html` to send a saved Studio page. ZIP uploads stay on `POST /api/cloud/v1/{siteId}` with a Cloud deploy key.
- `export` returns a Studio source ZIP, a Mobile Expo ZIP, or a Data JSON snapshot. It does not start an EAS build.
- `env_vars` lists, sets, updates, or deletes values for the owning account. Pass `product` and, except for the Code workspace, `projectId`. `exposure` is `public` or `secret`. Cloud publish writes public values into the static page. Private values stay out. There is no server runtime.
- `database` lists or creates Data tables and lists, creates, updates, or deletes records. Column types documented for Data are `text`, `number`, `boolean`, and `json`. `list_changes` polls rows updated after `since`. This is not app-user auth, a public bucket, Realtime, or Edge Functions.
- `files` lists, uploads, downloads, or deletes private Data files for a project this account owns. Uploads are base64, 1 MB maximum. This is not a public bucket.
- `eas_dispatch` forwards one Expo workflow dispatch with the customer's own token. The platform route takes that token, an Expo project UUID, a 40-character commit, and a workflow file name. Follow the tool schema for the argument names. The token is not stored. The starter ZIP is not uploaded. Xyvenra does not pay Expo. Call it only when the human asked to dispatch that existing workflow. `guidance` does not call it.
- `guidance` asks once about a real next step. Decline turns that path off. `continue` creates only the named step. It does not deploy or charge.
- `status` reports Code, Cloud, Data, Mobile, and Studio for the signed-in account.

## Safety

- Do not complete signup, key minting, billing, or account deletion for the user in a browser unless they have clearly asked and a supported path is already approved.
- Do not claim payments, hosted builds, or product features that are not in the tool results.
- Terms: https://xyvenra.com/terms. Privacy: https://xyvenra.com/privacy. Link those pages. Do not invent storage or privacy claims.
