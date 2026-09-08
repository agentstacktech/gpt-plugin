<!-- Human prose merged last by sync-gpt-instructions.mjs -->

When the user asks "what can AgentStack do?", summarize domains and point to `GET /mcp/actions` for the live catalog — do not quote stale action counts from memory.

**Anonymous bootstrap (no API key yet):** `projects.create_project_anonymous` returns `user_api_key` / `session_token` once plus neutral `bootstrap` metadata — configure the GPT Action header; do not echo secrets back to the user in chat.
