---
name: get-started
description: Use on AgentStack setup, Connect, sign-in, or the first project task. Call the bundled agentstack.execute tool. Do not ask the user to add a second MCP server.
---

# AgentStack

This plugin already includes one remote MCP server. Do not ask the user to add another one.

| Setting | Value |
|---------|--------|
| URL | `https://agentstack.tech/mcp` |
| Transport | `streamable-http` |
| Auth | The connector installs with the plugin. Calls work before sign-in. Account data uses Connect on this same connector. |
| Tool | `agentstack.execute` only |

## First action

Call the bundled tool. Do not start by telling the user the connector is missing.

```json
{
  "tool": "auth.get_profile",
  "params": {},
  "context": {}
}
```

- A profile means the connector is installed. Continue with `projects.get_projects`, then `agents.work_next` with `claim: false` and that `context.project_id`.
- Unauthorized or an anonymous profile means the connector is installed and the user is not signed in. Ask them to open **this plugin's AgentStack connector** and choose **Connect**. After sign-in, call `auth.get_profile` again.
- If the host reports that this plugin's connector is disabled, ask them to enable the AgentStack connector that is already on the plugin, then start a new chat. Do not send them to **Add custom MCP server**. A second server is a different connector and leaves this plugin without tools.

## Later calls

One action per call. After the user names a project, keep its id on `context.project_id`. Unfamiliar writes: `discovery.search` or `GET /mcp/actions` names only. Never invent action names.

Before delete, refund, role change, or API-key delete, restate the target and wait for a clear yes.

## Stay inside the request

- Do not start checkout, a new subscription, or a plan upgrade in this plugin.
- Do not print API keys, session tokens, or passwords.
- Empty, not found, or unauthorized means say that. Do not fill the gap with made-up rows.
