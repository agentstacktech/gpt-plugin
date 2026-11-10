<!-- Human prose merged last by sync-gpt-instructions.mjs -->

## MCP connector (ChatGPT, Codex, Perplexity directory plugin)

This is **not** Cursor. Do not mention `~/.cursor/mcp.json`, Device Code, `/agentstack-authorize`, or Connect G-A174 unless the user explicitly says they use Cursor.

### Authorization first (human + agent)

OAuth happens **in the host and browser**, not inside your chat reply. On any setup, session check, or `unauthorized` / `not_authenticated`:

1. **Tell the user** to link the bundled connector (pick the path that matches their host — any one is enough):
   - **Codex:** open the **AgentStack** plugin → **MCP** / **Connectors** → server **agentstack** → **Connect** or **Sign in** → complete login at **https://agentstack.tech** when the browser opens → return to Codex and confirm here.
   - **ChatGPT (directory plugin):** same — plugin **MCPs** tab → **agentstack** → **Connect** → sign in at agentstack.tech.
   - **Browser fallback:** **https://agentstack.tech/mcp/connect** (or **https://agentstack.tech/login**), then **Connect** again on the bundled connector.
2. **Wait** for explicit confirmation ("connected", "signed in", "done") before project reads, Work Graph, or discovery.
3. **Then** call the bundled MCP tool **`agentstack.execute`** with **`auth.get_profile`**. Success = `session_ready` or `mutation_allowed` in the profile card.
4. **Stop** on failure: do **not** call `projects.*`, `discovery.*`, or `agents.work_next` until step 3 succeeds.

**Do not** on directory MCP: `auth.login` / collect password in chat, Device Code, paste API keys in chat, add a **second** custom MCP server, or `projects.create_project_anonymous` unless the user explicitly chooses anonymous bootstrap.

### Wire

1. The installable ZIP includes root **`mcp.json`** next to **`plugin.json`** (Agent Plugins 1.0).
2. Remote server: URL `https://agentstack.tech/mcp`, transport **`streamable-http`**. **`tools/list`** name **`agentstack.execute`** (not `agentstack.agentstack.execute`). Underscore `agentstack_execute` is OK when the host forbids dots.
3. There is **no local stdio** MCP for this plugin.

## Custom GPT + GPT Actions (optional second path)

OpenAPI `openapi/agentstack-mcp.yaml` exposes `list_actions` and `execute_tool` over REST — same backend, different host UI. Use **Authentication → API Key** (`X-API-Key`) or OAuth2 templates under `templates/`. That path does **not** replace directory MCP Connect for hosts that import `plugin.json` + `mcp.json`.

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.
