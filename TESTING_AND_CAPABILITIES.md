# AgentStack GPT — Testing and capabilities

## Manual checklist

Two paths — do not mix steps ([MCP_CONNECT.md](MCP_CONNECT.md) · [GPT_QUICKSTART.md](GPT_QUICKSTART.md)).

### A. Directory plugin (MCP Connect)

- [ ] ZIP built with root `mcp.json` (`streamable-http`, `https://agentstack.tech/mcp`) — [DIRECTORY_SUBMIT.md](DIRECTORY_SUBMIT.md).
- [ ] Plugin installed in ChatGPT / Codex / Perplexity (not Actions-only).
- [ ] Human completed **Connect** on connector **agentstack** (OAuth at agentstack.tech).
- [ ] Agent called `auth.get_profile` via `agentstack.execute` before discovery or project reads.
- [ ] On 401/403: agent surfaces **Connect** again — not `auth.login` or Device Code in chat.
- [ ] `tools/list` shows one tool: `agentstack.execute`.
- [ ] Smoke prompt: “List my AgentStack projects” → `projects.get_projects`.
- [ ] Eval suite: [EVAL_PROMPTS.md](EVAL_PROMPTS.md) (directory section) · `evals/prompts.yaml`.

### B. Custom GPT Actions (OpenAPI)

- [ ] API key or OAuth configured on the GPT Action (anonymous project via `projects.create_project_anonymous` is **Actions-only** — prefer Connect on directory MCP).
- [ ] Custom GPT created in ChatGPT; Name and Description set.
- [ ] Action added: OpenAPI schema from `openapi/agentstack-mcp.yaml` pasted (or URL used).
- [ ] Authentication set to API Key (`X-API-Key`) or OAuth as appropriate.
- [ ] Instructions from `GPT_INSTRUCTIONS.md` pasted into the Custom GPT Instructions field.
- [ ] Custom GPT saved.

#### B1. Run one action

- [ ] In chat, ask: “Create a new project called Test via AgentStack.”
- [ ] GPT calls the action; response contains project id and keys (or a clear error).
- [ ] Confirm the reply does not log or repeat full API keys in plain text (only “key was returned” or similar).

#### B2. Optional checks

- [ ] “List my AgentStack projects” → `projects.get_projects` is called and results shown.
- [ ] “Get stats for project &lt;id&gt;” → `projects.get_stats` is called.
- [ ] On invalid or missing credential, GPT suggests checking Action Authentication and `MCP_CAPABILITY_MATRIX`.

### C. CORS and availability (Actions path)

- [ ] If ChatGPT reports network or CORS errors when calling the action, confirm `https://agentstack.tech/mcp` is reachable from the client and that the server allows requests from OpenAI (see main AgentStack repo and MCP docs if needed).

---

## What was tested

- Schema: OpenAPI 3.1 with `list_actions` (non-consequential) and `execute_tool` (`x-openai-isConsequential: true`); API Key/OAuth auth; compatibility noted in `openapi/SCHEMA_VALIDATION.md`.
- Instructions: Context, steps, and notes aligned with live MCP action names; reference to `MCP_CAPABILITY_MATRIX` for full list.
- Directory MCP: Connect ladder and `evals/prompts.yaml` (`auth_connect` → `auth.get_profile`).

---

## Typical scenarios

| User request | Directory MCP (`agentstack.execute`) | Custom GPT Actions |
|--------------|----------------------------------------|--------------------|
| After Connect | `auth.get_profile` first | N/A (credential on Action) |
| “List my projects” | `projects.get_projects` | `execute_tool` → `projects.get_projects` |
| “Create a project called X” | Signed-in project APIs (or explicit anon only if user refuses Connect) | `projects.create_project_anonymous` or account flow |
| “What can AgentStack do?” | `discovery.list` | `list_actions` then summarize |
| “Stats for project 1025” | `projects.get_stats` | `execute_tool` → `projects.get_stats` |

Full tool names: [MCP_CAPABILITY_MATRIX](https://github.com/agentstacktech/AgentStack/blob/master/docs/MCP_CAPABILITY_MATRIX.md) · live `GET https://agentstack.tech/mcp/actions`.

Eval prompts: [EVAL_PROMPTS.md](EVAL_PROMPTS.md).

---

## Latest Local Smoke Snapshot

2026-05-11:

- `node provided_plugins/scripts/validate-all-plugins.mjs` — passed with 3 warnings for Cursor placeholder screenshots.
- GPT OpenAPI schema was checked by the shared validator for OpenAPI 3.1, `execute_tool`, and `x-openai-isConsequential`.
