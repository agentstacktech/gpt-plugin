# AgentStack GPT — Quick Start (ChatGPT Custom GPT)

Get AgentStack working in ChatGPT in a few steps.

**Flow:** Create an anonymous project (no account) → get API key → add key in Custom GPT Action → use 60+ tools in chat. The MCP server allows `GET /mcp/tools` and `projects.create_project_anonymous` without X-API-Key so you can get a key first.

## Step 1: Choose authentication mode

You can connect AgentStack to Custom GPT in two modes:

- **Mode A — API Key** (fastest setup, service-to-service style)
- **Mode B — OAuth2** (user sign-in via AgentStack account)

---

## Mode A — API Key

### A1. Get an API key

**Option A — No account (try first):**

1. Create an anonymous project to get keys (no signup). From a terminal:

```bash
curl -X POST https://agentstack.tech/mcp/tools/projects.create_project_anonymous \
  -H "Content-Type: application/json" \
  -d '{"params": {"name": "My GPT Project"}}'
```

2. From the JSON response, copy `user_api_key` (or `api_key` / `project_api_key`) — you will use it as the API key in Step 3.

**Option B — With account:**

1. Sign in at [AgentStack](https://agentstack.tech) and create a project.
2. In the project settings, create an API key.
3. Use that key in Step 3.

### A2. Create a Custom GPT

1. In ChatGPT, go to **Explore** → **Create a GPT** (or [chat.openai.com](https://chat.openai.com) → Create).
2. In **Configure**, set **Name** (e.g. "AgentStack") and **Description** (e.g. "Create and manage AgentStack projects, API keys, and stats via the AgentStack backend.").

### A3. Add the Action

1. In the Custom GPT editor, open **Actions**.
2. Under **Schema**, paste the contents of `openapi/agentstack-mcp.yaml` from this repo, or use a URL if you host the schema (e.g. raw GitHub link to the YAML file).
   - If pasting: ensure the schema uses server URL `https://agentstack.tech` and path `POST /mcp` as in the provided file.
3. Under **Authentication**, choose **API Key**.
   - **Header name:** `X-API-Key`
   - **Key:** paste the API key from Step 1.

### A4. Paste instructions

1. Open **Instructions** in the Custom GPT editor.
2. Copy the full text from [GPT_INSTRUCTIONS.md](GPT_INSTRUCTIONS.md) (the block under "Copy the block below") and paste it into the Instructions field.

### A5. Save and use

1. Save the Custom GPT (e.g. "Only me" or share as needed).
2. In chat, try:
   - "Create a new project called Test App"
   - "List my AgentStack projects"
   - "Get stats for project 1025"

The GPT will call the AgentStack action with the right tool name and params.

---

## Mode B — OAuth2 (AgentStack as IdP)

Use this mode if you want users to click **Sign in with AgentStack** in your Custom GPT.

### B1. Prepare OAuth client in AgentStack

1. Create or request OAuth client credentials (`client_id`, `client_secret`) in AgentStack ecosystem.
2. Add your GPT callback URI to allowed redirect URIs, e.g.:
   - `https://chat.openai.com/aip/g-<GPT_ID>/oauth/callback`
   - or approved wildcard for trusted ecosystem clients.

### B2. Configure Action authentication

1. In Custom GPT editor → **Actions** → use the same schema `openapi/agentstack-mcp.yaml`.
2. Under **Authentication**, choose **OAuth** and set:
   - **Authorization URL:** `https://agentstack.tech/api/oauth2/authorize`
   - **Token URL:** `https://agentstack.tech/api/oauth2/token`
   - **Client ID / Secret:** from B1
3. Save and test sign-in in the GPT chat.

## Tool justification (OpenAI app approval)

When submitting your app, OpenAI may ask you to justify the **annotations** for each tool (readOnlyHint, openWorldHint, destructiveHint, idempotentHint). Copy-paste text for each tool from:

- **[TOOL_JUSTIFICATIONS.md](TOOL_JUSTIFICATIONS.md)** — one justification block per tool; use the "Justification" paragraph for the tool name that matches the form.

## Full tool list and docs

For all 60+ tools (Projects, Auth, Payments, Scheduler, Analytics, Rules, Webhooks, Notifications, Wallets), see:

- [MCP Server Capabilities](https://github.com/agentstack/agentstack/blob/main/docs/MCP_SERVER_CAPABILITIES.md) (in the AgentStack repo)

## Self-hosted MCP

If you run the MCP server yourself, change the **server URL** in the OpenAPI schema (or use a schema that points to your server, e.g. `https://your-host/mcp`). Authentication (API key in header `X-API-Key`) is still required.

---

## Troubleshooting: "Tool scan failed: Internal service error"

When adding or verifying the Action, OpenAI may show **"Tool scan failed: Internal service error"**. This is a known issue; see [OpenAI Community](https://community.openai.com/t/error-scan-tools-mcp-server-in-submission-form/1373481). Our MCP server returns **200 OK** on the full verification sequence (initialize, tools/list, notifications/initialized), so the failure can be intermittent on OpenAI’s side.

**Live check:** From the repo you can run the scanner-sequence script to confirm the server responds as expected: `python agentstack-core/scripts/mcp_gpt_scan_live_test.py` (optionally with a different base URL).

**What to do:**

1. **Retry** — Run verification again; it often succeeds on a second or third try.
2. **Server URL** — Use exactly **`https://agentstack.tech/mcp`** as the MCP/server base. Do not add paths like `/tools` or `/mcp/tools`.
3. **OAuth (Mode B)** — If using OAuth for the Action, set:
   - **Authorization URL:** `https://agentstack.tech/mcp/.well-known/oauth-authorize`
   - **Token URL:** `https://agentstack.tech/mcp/.well-known/oauth-token`
   Use the same Client ID/Secret as for the ecosystem; ensure your GPT’s callback URI is in the allowed redirect URIs.
