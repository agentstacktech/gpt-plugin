---
name: get-started
description: Use on AgentStack setup, Connect, sign-in, Codex/ChatGPT authorization, or first MCP call. Guide the human through host Connect/OAuth first; then auth.get_profile on agentstack.execute. Never Device Code or a second MCP server.
---

# AgentStack — connect and authorize

This plugin ships **one** remote MCP server. Do not ask the user to add another.

| Setting | Value |
|---------|--------|
| URL | `https://agentstack.tech/mcp` |
| Transport | `streamable-http` (Codex: `http` + `oauth_resource` in `.mcp.json`) |
| Tool | `agentstack.execute` only (`tools/list` name) |
| Auth | **Host Connect/OAuth** on the bundled **agentstack** connector — not chat login |

## Step 0 — human links account (before any MCP work)

You **cannot** finish OAuth from chat. Guide the user:

| Host | What they tap |
|------|----------------|
| **Codex** | AgentStack plugin → **MCP** / **Connectors** → **agentstack** → **Connect** or **Sign in** |
| **ChatGPT** | Plugin → **MCPs** → **agentstack** → **Connect** |
| **Fallback** | Open **https://agentstack.tech/mcp/connect** or **https://agentstack.tech/login**, sign in, then **Connect** again on the bundled connector |

Wait until they say they connected. Do **not** use `auth.login`, Device Code, API keys in chat, or a second MCP server on this path.

## Step 1 — agent verifies session

Call **`agentstack.execute`** (not `agentstack.agentstack.execute`) with:

```json
{
  "steps": [{ "id": "auth", "action": "auth.get_profile", "params": {} }],
  "context": {}
}
```

- **`session_ready`** or **`mutation_allowed`** → continue: `projects.get_projects`, bind `context.project_id`, `agents.work_next` with `claim: false`.
- **`unauthorized` / `not_authenticated`** → repeat Step 0; read `auth_choices` in the error if present.
- Connector disabled → enable the **bundled** AgentStack connector on this plugin; new chat; never "Add custom MCP server".

Anonymous `projects.create_project_anonymous` only after the user **explicitly** refuses sign-in.

## Later calls

One domain action per turn when possible. Keep `context.project_id` on the active project. Confirm before delete, refund, role change, or API-key delete.

## Stay inside the request

- No checkout, subscriptions, or plan upgrades in this plugin.
- Never print API keys, session tokens, or passwords.
- Empty / not found / unauthorized — say so; do not invent data.
