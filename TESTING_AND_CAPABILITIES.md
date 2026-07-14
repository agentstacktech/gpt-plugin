# AgentStack GPT — Testing and capabilities

## Manual checklist

Use this to verify the GPT integration end-to-end.

### 1. Create Custom GPT (per Quick Start)

- [ ] API key or OAuth configured (anonymous project via `projects.create_project_anonymous`, account dashboard, or OAuth where enabled for the GPT Action).
- [ ] Custom GPT created in ChatGPT; Name and Description set.
- [ ] Action added: OpenAPI schema from `openapi/agentstack-mcp.yaml` pasted (or URL used).
- [ ] Authentication set to API Key (`X-API-Key`) or OAuth as appropriate.
- [ ] Instructions from `GPT_INSTRUCTIONS.md` pasted into the Custom GPT Instructions field.
- [ ] Custom GPT saved.

### 2. Run one action

- [ ] In chat, ask: “Create a new project called Test via AgentStack.”
- [ ] GPT calls the action; response contains project id and keys (or a clear error).
- [ ] Confirm the reply does not log or repeat full API keys in plain text (only “key was returned” or similar).

### 3. Optional checks

- [ ] “List my AgentStack projects” → `projects.get_projects` is called and results shown.
- [ ] “Get stats for project &lt;id&gt;” → `projects.get_stats` is called.
- [ ] On invalid or missing credential, GPT suggests checking Action Authentication and `MCP_CAPABILITY_MATRIX`.

### 4. CORS and availability

- [ ] If ChatGPT reports network or CORS errors when calling the action, confirm `https://agentstack.tech/mcp` is reachable from the client and that the server allows requests from OpenAI (see main AgentStack repo and MCP docs if needed).

---

## What was tested

- Schema: OpenAPI 3.1 with `list_actions` (non-consequential) and `execute_tool` (`x-openai-isConsequential: true`); API Key/OAuth auth; compatibility noted in `openapi/SCHEMA_VALIDATION.md`.
- Instructions: Context, steps, and notes aligned with live MCP action names; reference to `MCP_CAPABILITY_MATRIX` for full list.

---

## Typical scenarios

| User request | Expected action |
|--------------|-----------------|
| “Create a project called X” | `execute_tool` with `tool: projects.create_project_anonymous`, `params: { "name": "X" }`. |
| “List my projects” | `execute_tool` with `tool: projects.get_projects`, `params: {}`. |
| “Stats for project 1025” | `execute_tool` with `tool: projects.get_stats`, `params: { "project_id": 1025 }`. |
| “What can AgentStack do?” | Use `list_actions` if needed; answer describing projects, auth, rules, buffs, payments, agents, storage, support, etc., and link to `MCP_CAPABILITY_MATRIX`. |

---

## Full tool list and docs

Exact tool names and parameters:

- **Capability Matrix** in the AgentStack repo: [docs/MCP_CAPABILITY_MATRIX.md](https://github.com/agentstacktech/AgentStack/blob/master/docs/MCP_CAPABILITY_MATRIX.md)
- Live list: `GET https://agentstack.tech/mcp/actions`
- Eval prompts: [EVAL_PROMPTS.md](EVAL_PROMPTS.md)

## Latest Local Smoke Snapshot

2026-05-11:

- `node provided_plugins/scripts/validate-all-plugins.mjs` — passed with 3 warnings for Cursor placeholder screenshots.
- GPT OpenAPI schema was checked by the shared validator for OpenAPI 3.1, `execute_tool`, and `x-openai-isConsequential`.
