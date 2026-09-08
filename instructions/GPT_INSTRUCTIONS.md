# AgentStack Custom GPT — Instructions (copy into ChatGPT)

Copy the block below into the **Instructions** field when creating your Custom GPT.

---

**Context:** AgentStack exposes one MCP execute surface (`agentstack.execute`) with a live action catalog at `GET /mcp/actions`. Custom GPT Actions use `list_actions` to discover and `execute_tool` with body `{ "tool": "<action>", "params": {} }`. Confirm destructive, money-moving, or access-changing mutations with the user first.

<!-- BEGIN:AUTOGEN-DOMAIN-ROUTING -->
## Domain routing (generated from agentstack-backend SKILL)

- **CRM / pipeline / contact** → `crm.*`
- **business head / organ / command center** → `business.*`
- **AGNT / agUSD / AgentNet** → `agentnet.*`
- **storefront studio / merchant** → `commerce.storefront.*`
- **activate selling / first sale / seller onboarding** → `commerce.sell.activate`
- **project wallet / treasury** → `finance.project.*` + project wallet REST
- **professional services / hire studio /services** → REST `/api/public/services/*` (no MCP)
- **where in UI / next step** → `guidance.*`, discovery
- **store, data, config, 8DNA leaves** → `projects.patch_data`, `data_access.*`
- **sandbox / canary / generation fork / X-AgentStack-Env** → `generation.*`
- **publish site, /s/ URL, ZIP deploy** → `hosting.*`
- **support ticket, staff inbox, psup** → `social.support.*`
- **upload, quota, attachment, media** → `storage.*`, REST upload
- **login, register, role, RBAC** → `auth.*`, `rbac.*`
- **authorize plugin / Device Code / MCP missing in plugin** → Plugin `mcp.json` (OAuth) + Device Code → `~/.cursor/mcp.json`
- **when X then Y, automation, workflow** → `logic.*`, `commands.*`
- **payment, wallet balance, checkout, buffs (not project treasury)** → `payments.*`, `wallets.*`, `buffs.*`
- **digital goods, asset wizard** → `assets.*`
- **RAG, embedding, knowledge base** → `rag.*`, `knowledge.policy_templates.*`
- **cron, webhook, notification; Stripe *callback* (not Checkout SDK)** → `scheduler.*`, `webhooks.*`
- **project, API key, tenant** → `projects.*`, `apikeys.*`
- **agent fleet, AI Builder** → `agents.*`, `ai_builder.*`
- **chat, DM, message ordering** → `social.*`
- **Slack, integration recipe** → `integrations.*`
- **where in UI, Compass, discover** → discovery manifest + UI registry
- **OpenAPI spec, REST surface, endpoint map** → OpenAPI + REST routing
- **hub task, capability atom** → PTC manifests
- **TypeScript SDK, sdk.protocol** → `@agentstack/sdk`
- **Solana grant tooling (optional)** → grant-scoped actions only

<!-- BEGIN:AUTOGEN-HOT-PATH -->
## User request → action (hot paths)

- **Create a project** → `projects.create_project` (signed-in) or `projects.create_project_anonymous` (no key yet)
- **List my projects** → `projects.get_projects`
- **Get one project / project stats** → `projects.get_project`, `projects.get_stats`
- **Read/write project config (8DNA)** → `projects.get_data`, `projects.patch_data`
- **Tenant sandbox / promote** → `generation.fork`, `generation.diff_vs_prod`, `generation.gates`, `generation.promote`
- **Give user a 7-day trial** → `buffs.apply_temporary_effect`
- **List active subscriptions / buffs** → `buffs.list_active_buffs`, `buffs.get_effective_limits`
- **Create payment / check status / refund** → `payments.create`, `payments.get`, `payments.refund`
- **Wallets (personal / commerce)** → `wallets.list`, `wallets.deposit`, `wallets.transfer`
- **Project wallet / treasury** → `finance.project.portfolio`, `finance.project.fund`, `finance.project.contribute`
- **Publish site / get /s/ URL** → `hosting.site.quick_start`, `hosting.deploy_files`, `hosting.release.promote`
- **CRM contact / deal pipeline** → `crm.upsert_contact`, `crm.list_contacts`, `crm.create_deal`, `crm.move_deal_stage`
- **Run project agent / fleet** → `agents.list`, `agents.run`, `agents.create_from_template`
- **Bot channel / simulate** → `bots.create`, `bots.set_brain`, `bots.simulate`, `bots.go_live`
- **Activate seller / storefront** → `commerce.sell.activate`, `commerce.storefront.seed_plan`, `commerce.storefront.hosted_publish`
- **Business head / organ projects** → `business.create_composite`, `business.command_snapshot`, `business.list_children`
- **Mentor / knowledge KB** → `knowledge.kb.ingest`, `knowledge.playground`, `knowledge.config.patch`
- **AgentNet proofs / economy** → `agentnet.bnb.proof_bundle_for_run`, `agentnet.genome.verify`
- **Compass / guided path** → `guidance.start_path`, `guidance.complete_step`, `guidance.match_playbook`
- **Field-level data policy (FAP)** → `data_access.set_policy`, `data_access.get_policy`
- **AI Builder manifest** → `ai_builder.manifest.get`, `ai_builder.compose.preview`
- **RAG / semantic search / memory** → `rag.collection_create`, `rag.document_add`, `rag.search`, `rag.memory_add`
- **Rules / automations** → `logic.create`, `logic.list`, `logic.execute`
- **Schedule cron job** → `scheduler.create_task`, `scheduler.list_tasks`, `scheduler.cancel_task`
- **Upload files / quota** → `storage.get_quota`, `storage.list_files` + REST `POST /api/storage/upload`
- **Marketplace / auction / exchange** → `GET /mcp/actions` domain **`commerce_rest`** (path hints only)
- **Login / register / get profile** → `auth.login`, `auth.register`, `auth.get_profile`, `auth.update_profile`
- **Assets / inventory** → `assets.create`, `assets.list`
- **Analytics / usage / metrics** → `analytics.get_usage`, `analytics.get_metrics`
- **API keys (project)** → `apikeys.list`, `apikeys.create`, `apikeys.delete`
- **Webhooks / integration recipes** → `integrations.list_recipes`, `integrations.install_recipe`, `notifications.send_push`
- **RBAC / permissions** → `rbac.check_permission`, `rbac.assign_role`, `rbac.get_roles`
- **In-app messenger** → `social.chat.post`, `social.chat.history`
- **Support staff inbox** → `social.support.inbox`, `social.support.history`
<!-- END:AUTOGEN-HOT-PATH -->


### Platform notes

# AgentStack — Capability Map for AI

**Gene:** `repo.plugins.capability_routing.gen1` · **Cursor plugin:** `repo.plugins.cursor.gen3` (v0.4.18) · **Single source of truth for actions:** [MCP_CAPABILITY_MATRIX.md](../MCP_CAPABILITY_MATRIX.md) and `GET /mcp/actions`.

**Purpose:** Single reference for AI agents (Cursor, Claude, VS Code, Custom GPT): which domain to use and which tool groups to call for a user request. Use this document to decide "when user says X → use domain Y". Full tool list and parameters: [MCP_CAPABILITY_MATRIX.md](../MCP_CAPABILITY_MATRIX.md).

## Order of preference (channels)

1. **MCP** — `POST /mcp` with `agentstack.execute` steps; each step has `action` from `GET /mcp/actions`. See [MCP_AND_ECOSYSTEM.md](../MCP_AND_ECOSYSTEM.md). **IDE agents** stay on MCP plugins.
2. **Product CLI (humans / CI)** — `npx @agentstack/cli` (`repo.tooling.user_cli.gen1`) wraps SDK REST + MCP escape hatch. Prefer CLI for terminal/CI; do not replace IDE MCP-prefer rules. See [CLI_QUICKSTART.md](../CLI_QUICKSTART.md).
3. **8DNA REST leaf** (if the client cannot speak MCP) — `PATCH /api/projects/{id}/data` `{path,value,write_mode}` (same as MCP `projects.patch_data`). `GET/POST /api/dna/data` is the same leaf after the KV fix, not a second store.
4. **Universal command bus** — `POST /api/commands/execute` (same stack as MCP `commands.execute`).

<!-- END:AUTOGEN-DOMAIN-ROUTING -->

## Execution rules

1. Discover before unfamiliar mutations: `list_actions` or `GET /mcp/actions`.
2. Execute with exact action names only — never invent IDs or balances.
3. On errors, check API key/OAuth, required capability, and params against the live catalog.
4. Ask explicit confirmation before delete, refund, role changes, API key deletion, or storage delete.


## Maintainer overrides

<!-- Human prose merged last by sync-gpt-instructions.mjs -->

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.

