# AgentStack GPT — Testing and capabilities

## Manual checklist

Use this to verify the GPT integration end-to-end.

### 1. Create Custom GPT (per Quick Start)

- [ ] API key obtained (anonymous project via `curl` to `POST https://agentstack.tech/mcp` with `projects.create_project_anonymous`, or from account).
- [ ] Custom GPT created in ChatGPT; Name and Description set.
- [ ] Action added: OpenAPI schema from `openapi/agentstack-mcp.yaml` pasted (or URL used).
- [ ] Authentication set to API Key, header `X-API-Key`, key pasted.
- [ ] Instructions from `GPT_INSTRUCTIONS.md` pasted into the Custom GPT Instructions field.
- [ ] Custom GPT saved.

### 2. Run one action

- [ ] In chat, ask: “Create a new project called Test via AgentStack.”
- [ ] GPT calls the action; response contains project id and keys (or a clear error).
- [ ] Confirm the reply does not log or repeat full API keys in plain text (only “key was returned” or similar).

### 3. Optional checks

- [ ] “List my AgentStack projects” → `projects.get_projects` is called and results shown.
- [ ] “Get stats for project &lt;id&gt;” → `projects.get_stats` is called.
- [ ] On invalid or missing API key, GPT suggests checking Action Authentication and MCP_SERVER_CAPABILITIES.

### 4. CORS and availability

- [ ] If ChatGPT reports network or CORS errors when calling the action, confirm `https://agentstack.tech/mcp` is reachable from the client and that the server allows requests from OpenAI (see main AgentStack repo and MCP docs if needed).

---

## What was tested

- Schema: OpenAPI 3.1 with one operation `execute_tool`, API Key auth; compatibility noted in `openapi/SCHEMA_VALIDATION.md`.
- Instructions: Context, steps, and notes aligned with projects and MCP tool names; reference to MCP_SERVER_CAPABILITIES for full list.

---

## Typical scenarios

| User request | Expected action |
|--------------|-----------------|
| “Create a project called X” | `execute_tool` with `tool: projects.create_project_anonymous`, `params: { "name": "X" }`. |
| “List my projects” | `execute_tool` with `tool: projects.get_projects`, `params: {}`. |
| “Stats for project 1025” | `execute_tool` with `tool: projects.get_stats`, `params: { "project_id": 1025 }`. |
| “What can AgentStack do?” | Answer describing projects, auth, rules, buffs, payments, etc., and link to MCP_SERVER_CAPABILITIES. |

---

## Full tool list and docs

Exact tool names and parameters:

- **MCP Server Capabilities** in the AgentStack repo: [docs/MCP_SERVER_CAPABILITIES.md](https://github.com/agentstacktech/AgentStack/blob/main/docs/MCP_SERVER_CAPABILITIES.md)
- Live list (with API key): `GET https://agentstack.tech/mcp/tools`
