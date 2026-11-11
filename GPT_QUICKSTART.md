# AgentStack GPT — Quick Start

Get AgentStack working in ChatGPT in a few steps.

**Preferred flow (directory MCP):** import the plugin ZIP → human **Connect** on **agentstack** → agent calls `auth.get_profile` → bind `context.project_id` → `agents.work_next` / `agentstack.execute`. Details: [MCP_CONNECT.md](MCP_CONNECT.md).

**Custom GPT Actions** (below) is a second path — API key or ecosystem OAuth on your own GPT via OpenAPI; it does not replace directory Connect.

**Anonymous project bootstrap** (`projects.create_project_anonymous`) is **Actions-only** — for directory MCP, use **Connect** instead unless the user explicitly refuses sign-in ([MCP_CONNECT.md](MCP_CONNECT.md)).

**Full map (ChatGPT + Gemini, all integration paths):** [docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md)

---

## Directory plugin — Connect first (ChatGPT, Codex, Perplexity)

The installable package is `plugin.json` + `mcp.json` + `skills/get-started/`. Build and upload steps: [DIRECTORY_SUBMIT.md](DIRECTORY_SUBMIT.md).

`mcp.json` declares one remote server:

| Field | Value |
|-------|--------|
| Transport | Streamable HTTP (`streamable-http`) |
| URL | `https://agentstack.tech/mcp` |
| Auth | Installed with the plugin. Account linking uses the server's OAuth discovery, not fields in this file. |

Hosts that only offer a **local** connector cannot use that option for this plugin. Local stdio is not in the package. On Perplexity, import the GitHub repo or the ZIP, then choose **Remote** and connect. Sign in with AgentStack when the host opens OAuth. Do not paste an API key into chat.

1. Upload/install the ZIP ([DIRECTORY_SUBMIT.md](DIRECTORY_SUBMIT.md)).
2. Open the host **MCP** / **MCPs** tab → connector **agentstack** → **Connect** → sign in at **https://agentstack.tech**.
3. In chat: confirm Connect done → probe with `auth.get_profile` before discovery or project reads.

**“No MCP” in the host?** You may have set up **Custom GPT Actions only** (OpenAPI) without importing the directory ZIP. Actions and directory MCP are two paths — continue with [Custom GPT Actions](#custom-gpt-actions-openapi) below or switch to [MCP_CONNECT.md](MCP_CONNECT.md).

## Custom GPT Actions (OpenAPI)

Pick the path that matches your ChatGPT plan and audience — directory Connect above is recommended when the host supports the plugin ZIP:

| Path | Best for | Setup |
|------|----------|--------|
| **Custom GPT + GPT Actions** | Plus users, shareable GPT, fastest API-key test | Steps below + [`templates/custom-gpt-apikey.template.json`](./templates/custom-gpt-apikey.template.json) or [`custom-gpt-oauth.template.json`](./templates/custom-gpt-oauth.template.json) |
| **Native MCP Connector** (Developer Mode) | Business/Enterprise, JSON-RPC in chat | [`templates/chatgpt-mcp-connector.template.json`](./templates/chatgpt-mcp-connector.template.json) · [Guide §4.2](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md#42-chatgpt-mcp-connector-developer-mode) |

**Auth decision tree:**

```
Need browser OAuth for end users? → Custom GPT Mode B (ecosystem OAuth template)
Need native MCP in chat?         → Developer Mode (well-known OR X-API-Key header)
Fastest solo test?               → Custom GPT Mode A (API Key template)
```

**OpenAI API remote MCP** (your product on Responses API): [Guide §4.3](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md#43-openai-api-responses--remote-mcp)

---

## Step 1: Choose authentication mode

You can connect AgentStack to Custom GPT in two modes:

- **Mode A — API Key** (fastest setup, service-to-service style)
- **Mode B — OAuth2** (user sign-in via AgentStack account)

---

## Mode A — API Key

### A1. Get an API key

**Option A — No account (Actions-only; not for directory MCP Connect):**

1. Create an anonymous project to get keys (no signup). From a terminal:

```bash
curl -X POST https://agentstack.tech/mcp \
  -H "Content-Type: application/json" \
  -d '{"tool": "projects.create_project_anonymous", "params": {"name": "My GPT Project"}}'
```

2. From the JSON response, copy `user_api_key` (or `session_token` for Bearer) — shown once at the top level with neutral `bootstrap` metadata (header/field names only). Configure the Custom GPT Action auth header; do not rely on model memory.

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
   - If pasting: ensure the schema uses server URL `https://agentstack.tech` and path `POST /mcp` (`agentstack.execute`). See the main MCP docs for the JSON shape.
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

For the full generated action list (Projects, Auth, Payments, Scheduler, Analytics, Rules, Webhooks, Notifications, Wallets, Agents, Storage, Support, and more), see:

- [MCP Capability Matrix](https://github.com/agentstacktech/AgentStack/blob/master/docs/MCP_CAPABILITY_MATRIX.md) (in the AgentStack repo)

## Self-hosted MCP

If you run the MCP server yourself, change the **server URL** in the OpenAPI schema (or use a schema that points to your server, e.g. `https://your-host/mcp`). Authentication (API key in header `X-API-Key`) is still required.

---

## Troubleshooting: "Tool scan failed: Internal service error"

When adding or verifying the Action, OpenAI may show **"Tool scan failed: Internal service error"**. This is a known issue; see [OpenAI Community](https://community.openai.com/t/error-scan-tools-mcp-server-in-submission-form/1373481). Our MCP server returns **200 OK** on the full verification sequence (initialize, tools/list, notifications/initialized), so the failure can be intermittent on OpenAI’s side.

**Live check:** From the repo you can run the scanner-sequence script to confirm the server responds as expected: `python agentstack-core/scripts/mcp_gpt_scan_live_test.py` (optionally with a different base URL).

**What to do:**

1. **Retry** — Run verification again; it often succeeds on a second or third try.
2. **Server URL** — Use exactly **`https://agentstack.tech/mcp`** as the MCP/server base. Do not add paths like `/tools` or `/mcp/tools`.
3. **OAuth (Mode B)** — Custom GPT Actions use ecosystem OAuth:
   - **Authorization URL:** `https://agentstack.tech/api/oauth2/authorize`
   - **Token URL:** `https://agentstack.tech/api/oauth2/token`
   For **ChatGPT MCP Connector** (Developer Mode), use MCP well-known instead:
   - `https://agentstack.tech/mcp/.well-known/oauth-authorize`
   - `https://agentstack.tech/mcp/.well-known/oauth-token`
   Ensure your GPT callback URI is in allowed redirect URIs (`https://chat.openai.com/aip/g-*/oauth/callback`).

---

## Native MCP Connector (Developer Mode) — short path

1. ChatGPT → Settings → Security and login → **Developer mode** ON.
2. Settings → Plugins → **Create** → MCP server URL: `https://agentstack.tech/mcp`.
3. Auth: API Key header `X-API-Key`, or OAuth (well-known URLs above).
4. Expect **one tool** in the list: `agentstack.execute`.
5. In chat: **+** → More → select AgentStack.

Details, auth matrix, and troubleshooting: [MCP_CHATGPT_GEMINI_GUIDE_RU.md](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md).
