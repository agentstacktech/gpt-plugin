<!-- Human prose merged last by sync-gpt-instructions.mjs -->

## MCP connector (ChatGPT, Codex, Perplexity directory plugin)

This is **not** Cursor. Do not mention `~/.cursor/mcp.json`, Device Code, or Connect G-A174 unless the user explicitly says they use Cursor.

1. The installable ZIP / GitHub package includes root **`mcp.json`** next to **`plugin.json`** (Agent Plugins 1.0). Hosts discover MCP from that file — it is not listed inside `plugin.json`.
2. Auth in `mcp.json` is **mixed**: the bundled connector can be used before sign-in. Account data needs **Connect** on that same AgentStack connector (OAuth discovery, dynamic client registration). Do not paste endpoint overrides or an API key into chat.
3. Remote server: URL `https://agentstack.tech/mcp`, transport **`streamable-http`**, single tool **`agentstack.execute`**. There is **no local stdio** process to start.
4. If this plugin's connector is disabled, ask the user to enable the AgentStack connector already listed on the plugin and start a new chat. Do not tell them to add a second custom MCP server.

## Custom GPT + GPT Actions (optional second path)

OpenAPI `openapi/agentstack-mcp.yaml` exposes `list_actions` and `execute_tool` over REST — same backend, different host UI. Use **Authentication → API Key** (`X-API-Key`) or OAuth2 templates under `templates/`. That path does not replace directory MCP Connect for hosts that import `plugin.json` + `mcp.json`.

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.
