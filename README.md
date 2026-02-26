# AgentStack — GPT (OpenAI) integration

Integration of **AgentStack** with **ChatGPT** via **GPT Actions**: use a Custom GPT to create projects, manage API keys, get stats, and call 60+ MCP tools.

AgentStack is a full backend ecosystem: 8DNA hierarchical data, Rules Engine, Buffs (trials/subscriptions), Payments, and MCP tools for Projects, Auth, Scheduler, Analytics, Webhooks, Notifications, and Wallets.

## What’s in this repo

| Item | Description |
|------|-------------|
| **OpenAPI schema** | `openapi/agentstack-mcp.yaml` — one action (execute MCP tool) for GPT Actions; API Key auth. |
| **Instructions** | `GPT_INSTRUCTIONS.md` — copy into your Custom GPT Instructions field. |
| **Quick Start** | `GPT_QUICKSTART.md` — get API key, create Custom GPT, add Action, paste instructions. |
| **Artifact layout** | `ARTIFACTS.md` — where each file lives and what it’s for. |

## Quick Start

1. Get an API key (anonymous project or account) — see [GPT_QUICKSTART.md](GPT_QUICKSTART.md).
2. Create a Custom GPT and add an Action with the OpenAPI schema; set Authentication to API Key, header `X-API-Key`.
3. Paste the instructions from [GPT_INSTRUCTIONS.md](GPT_INSTRUCTIONS.md).
4. Use the GPT (e.g. “Create a project called Test”, “List my projects”).

## Documentation

- **This plugin:** [github.com/agentstacktech/gpt-plugin](https://github.com/agentstacktech/gpt-plugin)
- **Quick Start:** [GPT_QUICKSTART.md](GPT_QUICKSTART.md)
- **Full MCP tool list:** [MCP Server Capabilities](https://github.com/agentstack/agentstack/blob/main/docs/MCP_SERVER_CAPABILITIES.md) in the main AgentStack repo
- **Plugins index and comparison (Cursor, Claude, GPT, VS Code):** [docs/plugins/README.md](https://github.com/agentstack/agentstack/blob/main/docs/plugins/README.md) in the main repo

## License

MIT. See [LICENSE](LICENSE).
