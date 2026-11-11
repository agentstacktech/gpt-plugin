# AgentStack — GPT (OpenAI) integration

Installable Agent Plugin for **ChatGPT**, **Codex**, and other Agent Plugins hosts (including **Perplexity**). The connector is remote Streamable HTTP at `https://agentstack.tech/mcp`. There is no local stdio server.

The server exposes one MCP tool, `agentstack.execute`. Action names come from the live `/mcp/actions` catalog.

**Directory plugin (ChatGPT / Codex / Perplexity):** the human completes **Connect** on the bundled `agentstack` MCP connector (OAuth at agentstack.tech). The agent then calls `auth.get_profile`, binds `context.project_id`, and uses `agents.work_next` — not `auth.login` or Device Code in chat. See [MCP_CONNECT.md](MCP_CONNECT.md).

**Custom GPT Actions** (optional second path): API key or OAuth on your own GPT via OpenAPI — not the directory ZIP connector.

AgentStack is a full backend ecosystem: 8DNA hierarchical data, Rules Engine, Buffs (trials/subscriptions), Payments, and MCP tools for Projects, Auth, Scheduler, Analytics, Webhooks, Notifications, and Wallets.

## What’s in this repo

| Item | Description |
|------|-------------|
| **Remote MCP** | `mcp.json` at plugin root (sibling of `plugin.json`) — only `type: streamable-http` and URL `https://agentstack.tech/mcp`. Hosts install that server from this file. See [MCP_CONNECT.md](MCP_CONNECT.md). |
| **OpenAPI schema** | `openapi/agentstack-mcp.yaml` — one action (execute MCP tool) for GPT Actions; supports API Key and OAuth2 auth modes. |
| **Instructions** | `GPT_INSTRUCTIONS.md` — copy into your Custom GPT Instructions field. |
| **Quick Start** | [MCP_CONNECT.md](MCP_CONNECT.md) for directory Connect; `GPT_QUICKSTART.md` for Custom GPT Actions (API key / OAuth). |
| **Artifact layout** | `ARTIFACTS.md` — where each file lives and what it’s for. |

## Quick Start

### Directory plugin (recommended)

1. Install the plugin (ChatGPT, Codex, or Perplexity) from the directory ZIP — remote MCP at `https://agentstack.tech/mcp` ([DIRECTORY_SUBMIT.md](DIRECTORY_SUBMIT.md)).
2. **Connect** in the host UI (plugin MCP connectors → **agentstack** → Connect).
3. Agent: `auth.get_profile` → set `options.context.project_id` → `agents.work_next` ([MCP_CONNECT.md](MCP_CONNECT.md)).

### Custom GPT Actions (optional)

1. Get an API key or configure OAuth — [GPT_QUICKSTART.md](GPT_QUICKSTART.md).
3. Create a Custom GPT and add an Action with the OpenAPI schema; choose authentication mode:
   - API Key (`X-API-Key`) for service-style access.
   - OAuth2 (AgentStack as IdP) for end-user sign-in.
4. Paste the instructions from [GPT_INSTRUCTIONS.md](GPT_INSTRUCTIONS.md).
5. Use the GPT (e.g. “Create a project called Test”, “List my projects”).

## What you can do

With the Custom GPT and Action configured, you can ask for any action exposed by the generated MCP capability matrix via `agentstack.execute`. Example prompts by domain:

| Domain | Example prompts |
|--------|-----------------|
| **Projects** | "List my projects", "Get stats for my project", "Create a project named Test" |
| **8DNA / Data** | "Store project data at key config.theme", "Read user data" |
| **Rules Engine** | "Create a rule when user signs up", "List logic rules" |
| **Buffs** | "Give user a 7-day trial", "List active buffs" |
| **Payments** | "Create a payment", "Get wallet balance" |
| **Auth** | "Get my profile" (after directory Connect; Custom GPT Actions may use email OTP — not directory MCP) |
| **Scheduler, Analytics, Webhooks, Notifications, Wallets** | "Schedule a task", "Get analytics", "List webhooks" |

**Full tool list and parameters:** [MCP Capability Matrix](https://github.com/agentstacktech/AgentStack/blob/master/docs/MCP_CAPABILITY_MATRIX.md). **When to use which tool:** [CONTEXT_FOR_AI](https://github.com/agentstacktech/AgentStack/blob/master/docs/plugins/CONTEXT_FOR_AI.md) in the AgentStack repo.

## Documentation

- **This plugin:** [github.com/agentstacktech/gpt-plugin](https://github.com/agentstacktech/gpt-plugin)
- **Quick Start:** [GPT_QUICKSTART.md](GPT_QUICKSTART.md)
- **Full MCP tool list:** [MCP Capability Matrix](https://github.com/agentstacktech/AgentStack/blob/master/docs/MCP_CAPABILITY_MATRIX.md) in the main AgentStack repo
- **Plugins index and comparison (Cursor, Claude, GPT, VS Code):** [docs/plugins/README.md](https://github.com/agentstacktech/AgentStack/blob/master/docs/plugins/README.md) in the main repo

## Links

- **AgentStack:** [agentstack.tech](https://agentstack.tech)
- **LinkedIn:** [linkedin.com/company/agentstacktech](https://www.linkedin.com/company/agentstacktech/)
- **GitHub:** [github.com/agentstacktech](https://github.com/agentstacktech)

## License

MIT. See [LICENSE](LICENSE).
