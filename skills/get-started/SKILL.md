---
name: get-started
description: Use on AgentStack setup, Connect, sign-in, or the first project task. Authorize first, then call auth.get_profile through the bundled agentstack.execute tool. Do not ask the user to add a second MCP server.
---

# AgentStack

This plugin already includes one remote MCP server. Do not ask the user to add another one.

| Setting | Value |
|---------|--------|
| URL | `https://agentstack.tech/mcp` |
| Transport | `streamable-http` |
| Auth | The connector installs with the plugin. Calls work before sign-in. Account data uses Connect on this same connector. |
| Tool | `agentstack.execute` only (`tools/list` name) |

## First action: authorize and verify

Authorization is the first step. Do not call discovery, projects, Work Graph, or any
other action before the auth probe.

**Tool name:** use exactly what `tools/list` returns — `agentstack.execute`. Do **not**
call `agentstack.agentstack.execute` (connector id + tool id double-prefix). Underscore
form `agentstack_execute` is fine when the host forbids dots.

Arguments for the MCP `tools/call` (or host wrapper) are the execute body:

```json
{
  "steps": [{ "id": "auth", "action": "auth.get_profile", "params": {} }],
  "context": {}
}
```

If the host already wraps execute and asks only for the inner action:

```json
{
  "tool": "auth.get_profile",
  "params": {},
  "context": {}
}
```

- A profile with `session_ready` or `mutation_allowed` means authorization succeeded. Then call `projects.get_projects`, bind `context.project_id`, and call `agents.work_next` with `claim: false`.
- `unauthorized`, `not_authenticated`, or an anonymous profile means the connector is installed but the user is not signed in. Ask them to open **this plugin's AgentStack connector** and choose **Connect**. After sign-in, call `auth.get_profile` again before doing anything else.
- Do not silently create an anonymous project. Only call `projects.create_project_anonymous` after the user explicitly chooses anonymous bootstrap instead of signing in; preserve its returned `project_id`, `user_api_key`, and `session_token` without displaying secrets.
- If the host reports that this plugin's connector is disabled, ask them to enable the AgentStack connector that is already on the plugin, then start a new chat. Do not send them to **Add custom MCP server**. A second server is a different connector and leaves this plugin without tools.

## Later calls

One action per call. After the user names a project, keep its id on `context.project_id`. Unfamiliar writes: `discovery.search` or `GET /mcp/actions` names only. Never invent action names.

Before delete, refund, role change, or API-key delete, restate the target and wait for a clear yes.

## Stay inside the request

- Do not start checkout, a new subscription, or a plan upgrade in this plugin.
- Do not print API keys, session tokens, or passwords.
- Empty, not found, or unauthorized means say that. Do not fill the gap with made-up rows.
