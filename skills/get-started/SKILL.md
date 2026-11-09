---
name: get-started
description: Use when the user wants an AgentStack project, app data, sign-in, a hosted site, file storage, or a rule. Call the connected MCP tool agentstack.execute and read action names from the live catalog before an unfamiliar write.
---

# AgentStack

AgentStack is the backend for the user's app.

## Directory plugin (native MCP)

This package ships **`mcp.json`** at the plugin root (next to `plugin.json`). The host loads it automatically — do not look for MCP inside `plugin.json`.

| Setting | Value |
|---------|--------|
| URL | `https://agentstack.tech/mcp` |
| Transport | `streamable-http` |
| Auth | OAuth with dynamic client registration (Connect in the host UI) |
| Tool | `agentstack.execute` only |

There is **no local stdio** server. If the user only built a Custom GPT with OpenAPI Actions, that is a different setup — point them to `GPT_QUICKSTART.md` for API Key / OAuth2 Actions auth.

## How to call it

1. If the user is not connected, ask them to use **Connect** on the AgentStack plugin and sign in to AgentStack. Do not invent an API key or ask them to paste a password, card number, or one-time code into chat.
2. For an unfamiliar action, call `agentstack.execute` with a discovery action first and use only names the catalog returns.
3. Pass `tool` and `params` for one action per call. Keep `project_id` on follow-up calls when the user is working in one project.
4. Before a delete or any other irreversible change, restate the target and wait for a clear yes.
5. If the server returns not found, unauthorized, or empty, say that. Do not fill the gap with made-up rows.

## Stay inside the request

- Read and change only the account and project the user asked about.
- Do not start checkout, a new subscription, a credit purchase, or a plan upgrade. If they ask to buy or upgrade, say that is not available in this plugin.
- Do not print API keys, session tokens, or passwords from a response. Tell the user where the product shows a secret once, and do not repeat it.

## First tasks

- List projects.
- Create a project when they name one.
- Store or read a small config value they specify.
- Publish or inspect a hosted site when they ask for a public URL.
