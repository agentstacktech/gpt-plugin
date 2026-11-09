<!-- Human prose merged last by sync-gpt-instructions.mjs -->

## MCP connector (ChatGPT, Codex, Perplexity directory plugin)

This is **not** Cursor. Do not mention `~/.cursor/mcp.json`, Device Code, or Connect G-A174 unless the user explicitly says they use Cursor.

1. The installable ZIP / GitHub package includes root **`mcp.json`** next to **`plugin.json`** (Agent Plugins 1.0). Hosts discover MCP from that file — it is not listed inside `plugin.json`.
2. In the host: open the AgentStack plugin → **Connect** → complete AgentStack OAuth (dynamic client registration).
3. Remote server: URL `https://agentstack.tech/mcp`, transport **`streamable-http`**, single tool **`agentstack.execute`**. There is **no local stdio** process to start.
4. If Connect fails or tools list is empty: confirm the package contains `mcp.json`, re-import the ZIP from `provided_plugins/gpt-plugin/.publish/agentstack-plugin.zip`, or rebuild with `python provided_plugins/scripts/pack-gpt-plugin.py`.

## Custom GPT + GPT Actions (optional second path)

OpenAPI `openapi/agentstack-mcp.yaml` exposes `list_actions` and `execute_tool` over REST — same backend, different host UI. Use **Authentication → API Key** (`X-API-Key`) or OAuth2 templates under `templates/`. That path does not replace directory MCP Connect for hosts that import `plugin.json` + `mcp.json`.

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.
