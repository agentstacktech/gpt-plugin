---
name: get-started
description: Use on AgentStack setup, Connect, sign-in, or the first project task. The host must attach the remote MCP server before any tool call. Teach OAuth Connect, then agentstack.execute.
---

# AgentStack

The backend is one remote MCP server. There is no local process.

| Setting | Value |
|---------|--------|
| URL | `https://agentstack.tech/mcp` |
| Transport | `http` (Streamable HTTP on that URL) |
| Auth | OAuth, dynamic client registration |
| Tool | `agentstack.execute` only |

`plugin.json` does not embed the server. ChatGPT reads **`mcp.json`** and **`.mcp.json`** (same server). If the plugin card mentions MCP but the chat has no tools, the server was not attached.

## If MCP did not install

Tell the user to do this in ChatGPT, in order:

1. Open **Plugins** (or Settings → Apps / Connectors).
2. Choose **Add custom MCP server** (plus button). Do not paste an API key into chat.
3. Server URL: `https://agentstack.tech/mcp`. Authentication: **OAuth** (not a local/stdio connector).
4. Finish the AgentStack sign-in page. Scopes are `openid`, `tools:read`, `tools:invoke`.
5. Re-open the AgentStack plugin and confirm the connector is **enabled**. Start a **new** chat.

If Connect fails, say the server did not attach. Do not invent projects, keys, or balances.

## How to call the tool

One action per call:

```json
{
  "tool": "auth.get_profile",
  "params": {},
  "context": {}
}
```

After the user names a project, keep its id:

```json
{
  "tool": "agents.work_next",
  "params": { "claim": false },
  "context": { "project_id": 123 }
}
```

Order for a new session:

1. `auth.get_profile` — who is signed in. If unauthorized, stop and ask them to Connect again.
2. `projects.get_projects` — list projects. Ask which one if several.
3. `agents.work_next` with `claim: false` — next step on that project.
4. Unfamiliar writes: `discovery.search` or `GET /mcp/actions` names only. Never invent action names.

Before delete, refund, role change, or API-key delete, restate the target and wait for a clear yes.

## Stay inside the request

- Do not start checkout, a new subscription, or a plan upgrade in this plugin.
- Do not print API keys, session tokens, or passwords. A one-time secret is configured in the product UI, not repeated in chat.
- Empty, not found, or unauthorized means say that. Do not fill the gap with made-up rows.
