---
name: publish-with-xyvenra
description: Publish a static site on Xyvenra Cloud. Use when the user wants to put a landing page or HTML site online, create a Xyvenra Cloud site, or deploy with a platform API key. Also use for Xyvenra Data project and table setup through MCP.
---

# Publish with Xyvenra

Use this skill when the user wants to put a simple site or landing page online, create a Xyvenra Cloud site, deploy static HTML, or work with a Data project through the Xyvenra MCP at https://xyvenra.com/api/mcp.

## When not to use it

- Do not use Xyvenra Cloud as a Node, Next.js, or general app host. Cloud serves self-contained static HTML (180 KB or smaller for this MCP deploy tool).
- Do not try to create Studio, Mobile, or Code projects through this API; those are still in-app only.
- Never invent a key, scrape one from a file into the conversation, or print a platform API key. Never write a key into a committed file.

## Exact flow

A human must mint the key. Agents cannot complete signup or key creation in a browser.

1. If no platform API key is connected, tell the human to sign in at https://xyvenra.com/login (GitHub or Google) and mint a key at https://xyvenra.com/app/keys. The key is shown once, starts with `xyv_plat_`, and is sent as `Authorization: Bearer`. Wait until that key is connected to the MCP server.
2. Create the Cloud site with the `create_project` tool: `product` is `cloud`, provide a `name`, and a `slug` (1 to 48 characters, lowercase letters, numbers, and internal hyphens). Remember the returned site id.
3. Deploy self-contained HTML with the `deploy` tool, passing `siteId` and `html`. A successful platform or CLI or MCP HTML deploy publishes the public page.
4. Give the human the live URL: `https://xyvenra.com/sites/{slug}`. Prefer the `url` field returned by `deploy`.

## Limits and other tools

- Accounts can have at most three Cloud sites.
- `status` lists the key owner's Cloud sites and Data projects.
- For Data, call `create_project` with `product` `data` and a `name`, then `database` for `list_tables`, `create_table`, or `list_records`.
- `env_vars` is not available yet.
- ZIP or in-app Cloud deploys use a Cloud deploy key and keep the current published state. This MCP `deploy` path publishes the public page.

## Safety

- Do not complete signup, key minting, billing, or account deletion for the user in a browser unless they have clearly asked and a supported path is already approved.
- Do not claim payments, checkout, or products that are not in the tool results.
