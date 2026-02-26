# AgentStack GPT — Quick Start (ChatGPT Custom GPT)

Get AgentStack working in ChatGPT in a few steps.

## Step 1: Get an API key

**Option A — No account (try first):**

1. Create an anonymous project to get keys (no signup). From a terminal:

```bash
curl -X POST https://agentstack.tech/mcp \
  -H "Content-Type: application/json" \
  -d '{"tool": "projects.create_project_anonymous", "params": {"name": "My GPT Project"}}'
```

2. From the JSON response, copy `project_api_key` or `user_api_key` — you will use it as the API key in Step 3.

**Option B — With account:**

1. Sign in at [AgentStack](https://agentstack.tech) and create a project.
2. In the project settings, create an API key.
3. Use that key in Step 3.

## Step 2: Create a Custom GPT

1. In ChatGPT, go to **Explore** → **Create a GPT** (or [chat.openai.com](https://chat.openai.com) → Create).
2. In **Configure**, set **Name** (e.g. "AgentStack") and **Description** (e.g. "Create and manage AgentStack projects, API keys, and stats via the AgentStack backend.").

## Step 3: Add the Action

1. In the Custom GPT editor, open **Actions**.
2. Under **Schema**, paste the contents of `openapi/agentstack-mcp.yaml` from this repo, or use a URL if you host the schema (e.g. raw GitHub link to the YAML file).
   - If pasting: ensure the schema uses server URL `https://agentstack.tech` and path `POST /mcp` as in the provided file.
3. Under **Authentication**, choose **API Key**.
   - **Header name:** `X-API-Key`
   - **Key:** paste the API key from Step 1.

## Step 4: Paste instructions

1. Open **Instructions** in the Custom GPT editor.
2. Copy the full text from [GPT_INSTRUCTIONS.md](GPT_INSTRUCTIONS.md) (the block under "Copy the block below") and paste it into the Instructions field.

## Step 5: Save and use

1. Save the Custom GPT (e.g. "Only me" or share as needed).
2. In chat, try:
   - "Create a new project called Test App"
   - "List my AgentStack projects"
   - "Get stats for project 1025"

The GPT will call the AgentStack action with the right tool name and params.

## Full tool list and docs

For all 60+ tools (Projects, Auth, Payments, Scheduler, Analytics, Rules, Webhooks, Notifications, Wallets), see:

- [MCP Server Capabilities](https://github.com/agentstack/agentstack/blob/main/docs/MCP_SERVER_CAPABILITIES.md) (in the AgentStack repo)

## Self-hosted MCP

If you run the MCP server yourself, change the **server URL** in the OpenAPI schema (or use a schema that points to your server, e.g. `https://your-host/mcp`). Authentication (API key in header `X-API-Key`) is still required.
