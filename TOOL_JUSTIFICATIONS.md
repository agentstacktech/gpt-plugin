# Tool justifications for OpenAI app approval

AgentStack GPT Actions expose **two operations** only:

| Operation | OpenAPI | Purpose |
|-----------|---------|---------|
| `list_actions` | `GET /mcp/actions` | Read-only discovery of MCP action names and caps |
| `execute_tool` | `POST /mcp` | Execute one registered action by `{ "tool", "params" }` |

Copy the justification blocks below into OpenAI review forms.

---

## list_actions

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=true

**Justification:**  
Lists available AgentStack MCP actions from our backend catalog. Read-only; no mutations. Calls only `https://agentstack.tech/mcp/actions`. Same request returns the same catalog snapshot for a given registry version, so it is idempotent. Models should call this before unfamiliar mutations.

---

## execute_tool

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=true, idempotentHint=false

**Justification:**  
Executes a single registered AgentStack MCP action by name with parameters. Mutations (create project, payments, deletes) depend on the chosen action; the GPT must confirm destructive or money-moving operations with the user. Calls only our MCP API at `https://agentstack.tech/mcp`. Not globally idempotent — each call may create new resources. Discovery via `list_actions` keeps the action surface explicit and reviewable.
