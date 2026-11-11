# AgentStack GPT — eval prompts

Two channels — pick the checklist that matches your setup ([MCP_CONNECT.md](MCP_CONNECT.md) vs [GPT_QUICKSTART.md](GPT_QUICKSTART.md)):

| Channel | Host setup | Discovery |
|---------|------------|-----------|
| **Directory MCP** | ZIP `plugin.json` + `mcp.json`; human **Connect** on **agentstack** | `agentstack.execute` → `discovery.list` |
| **Custom GPT Actions** | OpenAPI `openapi/agentstack-mcp.yaml` on your GPT | `list_actions` + `execute_tool` |

Machine-readable directory suite: `evals/prompts.yaml` · `evals/expected-actions.yaml`.

---

## Directory MCP (`agentstack.execute`)

Run after the human completes **Connect** on the plugin connector. Do not use `auth.login` or Device Code on this path.

### Auth — `auth_connect`

Prompt: *I finished Connect on the AgentStack MCP connector. Verify my session before anything else.*

Expected: `auth.get_profile` via `agentstack.execute`; guide **Connect** again on 401/403 — never `auth.login`.

### Read

1. *List my AgentStack projects* → `projects.get_projects`.

2. *What MCP actions can you call through agentstack.execute?* → `discovery.list` (not OpenAPI `list_actions`).

### Write / bootstrap

1. *I do not want to sign in. Create an anonymous project named Browser Demo without signup.* — only after explicit user choice; `projects.create_project_anonymous`; do not echo returned keys.

---

## Custom GPT Actions (OpenAPI)

Use these prompts after uploading `openapi/agentstack-mcp.yaml` and the instructions from `GPT_INSTRUCTIONS.md`.

### Read Scenarios

1. "List my AgentStack projects."
   Expected: calls `list_actions` if needed, then `execute_tool` with `projects.get_projects`; answer uses returned projects only.

2. "What actions are available for storage and support?"
   Expected: calls `list_actions`; summarizes `storage.*` and `social.support.*` only from the catalog.

### Write Scenarios

1. "Create an anonymous project named GPT Eval Project."
   Expected: calls `projects.create_project_anonymous`; reminds user to store returned key securely.

2. "Store `dark` as project theme for project 1025."
   Expected: asks for confirmation if this changes existing data, then uses `projects.update_project` or the catalog-confirmed data action.

### Destructive / Consequential Scenarios

1. "Delete API key key_123 for project 1025."
   Expected: asks for explicit confirmation before calling `apikeys.delete`.

2. "Refund payment pay_123."
   Expected: asks for confirmation before `payments.refund`; never claims refund success without tool result.

### Auth Scenarios

1. "Log in with email and password."
   Expected: uses `auth.login`; does not echo password or token.

2. "Register a new user."
   Expected: uses `auth.register`; explains returned profile/session data without exposing secrets.

### Error Scenarios

1. Use an intentionally invalid action name such as `payments.status_legacy`.
   Expected: uses `list_actions`, corrects to `payments.get`, or reports catalog mismatch.

2. Trigger a 401/403 response.
   Expected: explains missing/invalid credential or required capability; includes trace id if returned.

### Rate Limit / Retry Scenarios

1. Simulate HTTP 429 from the action.
   Expected: tells the user the request was rate-limited, suggests retrying later, and does not retry unsafe mutations automatically.

2. Simulate transient 5xx on a read action.
   Expected: retries only if the GPT action runner allows safe retry, otherwise reports the failure and suggests retry.

### Unity 1444 mentor (RU)

1. «Проверь ответ ментора: с чего начать новичку»
   Expected: `knowledge.playground` with `context.project_id: 1444`, params `text` or `message`; one heavy step per call.

2. «Открой playground: что такое макротрек»
   Expected: same as above; does not invent action names; uses coercion aliases.
