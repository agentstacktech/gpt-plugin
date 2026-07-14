# AgentStack — GPT (OpenAI) integration

Integration of **AgentStack** with **ChatGPT** via **GPT Actions**: use a Custom GPT to create projects, manage API keys, get stats, and call one MCP tool (`agentstack.execute`) whose current actions come from the live `/mcp/actions` catalog.

AgentStack is a full backend ecosystem: 8DNA hierarchical data, Rules Engine, Buffs (trials/subscriptions), Payments, and MCP tools for Projects, Auth, Scheduler, Analytics, Webhooks, Notifications, and Wallets.

## What’s in this repo

| Item | Description |
|------|-------------|
| **OpenAPI schema** | `openapi/agentstack-mcp.yaml` — one action (execute MCP tool) for GPT Actions; supports API Key and OAuth2 auth modes. |
| **Instructions** | `GPT_INSTRUCTIONS.md` — copy into your Custom GPT Instructions field. |
| **Quick Start** | `GPT_QUICKSTART.md` — get API key, create Custom GPT, add Action, paste instructions. |
| **Artifact layout** | `ARTIFACTS.md` — where each file lives and what it’s for. |

## Quick Start

1. Get an API key (anonymous project or account) — see [GPT_QUICKSTART.md](GPT_QUICKSTART.md).
2. Create a Custom GPT and add an Action with the OpenAPI schema; choose authentication mode:
   - API Key (`X-API-Key`) for service-style access.
   - OAuth2 (AgentStack as IdP) for end-user sign-in.
3. Paste the instructions from [GPT_INSTRUCTIONS.md](GPT_INSTRUCTIONS.md).
4. Use the GPT (e.g. “Create a project called Test”, “List my projects”).

## What you can do

With the Custom GPT and Action configured, you can ask for any action exposed by the generated MCP capability matrix via `agentstack.execute`. Example prompts by domain:

| Domain | Example prompts |
|--------|-----------------|
| **Projects** | "List my projects", "Get stats for my project", "Create a project named Test" |
| **8DNA / Data** | "Store project data at key config.theme", "Read user data" |
| **Rules Engine** | "Create a rule when user signs up", "List logic rules" |
| **Buffs** | "Give user a 7-day trial", "List active buffs" |
| **Payments** | "Create a payment", "Get wallet balance" |
| **Auth** | "Get my profile", "Quick auth with email" |
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
