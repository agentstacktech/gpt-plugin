<!-- Human prose merged last by sync-gpt-instructions.mjs -->

## MCP connector (ChatGPT, Codex, Perplexity directory plugin)

This is **not** Cursor. Do not mention `~/.cursor/mcp.json`, Device Code, or Connect G-A174 unless the user explicitly says they use Cursor.

1. The installable ZIP / GitHub package includes root **`mcp.json`** next to **`plugin.json`** (Agent Plugins 1.0). Hosts discover MCP from that file — it is not listed inside `plugin.json`.
2. **Authorize first.** Before discovery, project reads, or Work Graph calls, call `agentstack.execute` with `auth.get_profile` after the host's **Connect/OAuth** flow. If it returns `unauthorized` or `not_authenticated`, stop and ask the user to authorize; do not continue with `projects.*`, `discovery.*`, or `agents.work_next`.
3. Remote server: URL `https://agentstack.tech/mcp`, transport **`streamable-http`**, single tool **`agentstack.execute`**. There is **no local stdio** process to start.
4. An anonymous project is only a fallback after the user explicitly chooses it: use `projects.create_project_anonymous`, preserve its returned credentials, and never print them in chat. If this plugin's connector is disabled, ask the user to enable the AgentStack connector already listed on the plugin and start a new chat. Do not tell them to add a second custom MCP server.

## Custom GPT + GPT Actions (optional second path)

OpenAPI `openapi/agentstack-mcp.yaml` exposes `list_actions` and `execute_tool` over REST — same backend, different host UI. Use **Authentication → API Key** (`X-API-Key`) or OAuth2 templates under `templates/`. That path does not replace directory MCP Connect for hosts that import `plugin.json` + `mcp.json`.

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.
