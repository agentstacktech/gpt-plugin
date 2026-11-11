# AgentStack MCP — ChatGPT / Codex / Perplexity

**Gene:** `repo.plugins.bundle.gen1` · Spec: [Agent Plugins 1.0](https://agent-plugins.org/) · Backend: `core.mcp.tools.gen1`

## What “provides MCP” means here

| Integration | Files | What the user sees |
|-------------|-------|-------------------|
| **Directory plugin** | Root `plugin.json` + root `mcp.json` + `.codex-plugin/plugin.json` + `.mcp.json` + `skills/` | ChatGPT/portable hosts use root `mcp.json` (`streamable-http`). Codex discovers only `.codex-plugin/plugin.json`, which points to `.mcp.json` (`http` + `oauth_resource`). Both files intentionally use the server name `agentstack`; this is the connector identity, not a second server. Skill icons: `interface.logo` in the Codex manifest plus `agents/openai.yaml` and `assets/icon.png`. |
| **Custom GPT Actions** | `openapi/agentstack-mcp.yaml` | REST `list_actions` / `execute_tool` — **not** the same UI as directory MCP |

If you only created a Custom GPT with Actions and skipped the directory ZIP, the host may show **no MCP connector** — that is expected. Either import the directory package or stay on the Actions path ([GPT_QUICKSTART.md](./GPT_QUICKSTART.md)).

## Directory package checklist

1. Build: `python provided_plugins/scripts/pack-gpt-plugin.py` → `provided_plugins/gpt-plugin/.publish/agentstack-plugin.zip`
2. ZIP root must include **`mcp.json`** (streamable-http, `https://agentstack.tech/mcp`)
3. Upload ZIP in the ChatGPT Plugins portal (org owner + developer verification)
4. On **MCPs** tab: connect server → OAuth sign-in to AgentStack
5. Rescan tools — expect one tool: `agentstack.execute` (never invent `agentstack.agentstack.execute`; that double-prefix is a host namespacing mistake — server accepts it as alias, but call the `tools/list` name)

## Auth

`mcp.json` does not embed OAuth settings. The Agent Plugins schema allows only `type` and `url` on the server; an `extensions` block makes ChatGPT ignore the server, so the plugin lists MCP and the MCP page stays empty. The live server advertises `noauth` plus `oauth2` on `agentstack.execute` and publishes discovery at:

- `https://agentstack.tech/mcp/.well-known/oauth-protected-resource`
- `https://agentstack.tech/mcp/.well-known/oauth-authorization-server`

The server answers `initialize` and `tools/list` without a token. Sign-in is the plugin connector's **Connect** button, not a second custom MCP server.

No API key in `mcp.json`. No fixed `Authorization` header.

## Codex

1. Install from `.codex-plugin/plugin.json` (entry) → `.mcp.json` (`http` + `oauth_resource`).
2. **Plugin → MCP Connectors → agentstack → Connect** — sign in at **https://agentstack.tech**.
3. Fallback: **https://agentstack.tech/mcp/connect**, then Connect again in Codex.
4. After the user confirms, call `agentstack.execute` → `auth.get_profile`. Do not use Device Code or `auth.login` on this path.

## First MCP action (all hosts)

1. Human completes **Connect/OAuth** on this plugin's **agentstack** connector (browser login at agentstack.tech).
2. Agent calls `agentstack.execute` with `auth.get_profile` before discovery, project reads, or Work Graph actions.
3. If unauthorized, guide Connect again — on **401/403** from `agentstack.execute`, read `auth_choices`, `pick_one`, and `auth_surface.client_auth_ladder`; surface one human step. Do not call `auth.login` or Device Code on directory MCP.
4. Only if the user explicitly chooses anonymous bootstrap, call `projects.create_project_anonymous`. Never display its returned credentials.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|----------------|-----|
| No MCP / “local only” | Old ZIP without `mcp.json`, or Actions-only setup | Rebuild ZIP; import directory package |
| `test_cases require exactly one MCP server` | Plugin-level review cases are combined with portable + Codex MCP manifests | Omit plugin-level `test_cases`; keep one server in each manifest |
| Plugin lists MCP, page has no AgentStack server | Codex loaded root `plugin.json` but did not find its required `.codex-plugin/plugin.json` entrypoint | Ship `.codex-plugin/plugin.json` → `.mcp.json`; server keys only the documented HTTP fields; restart Codex after ZIP install |
| Newspaper / shield icon after install | Skill lacks `agents/openai.yaml` icons | Ship `skills/get-started/agents/openai.yaml` and `assets/icon.png` |
| Empty tools list | Session not scoped | Retry Connect; probe `tools/list` via [mcp_connector_scan_live_test.py](../../agentstack-core/scripts/mcp_connector_scan_live_test.py) |
| Unauthorized after Connect | Connector still unsigned or stale token | Pick `plugin_connect_oauth` or `browser_connect_help` from `auth_choices`; re-probe with `auth.get_profile` |
| Works in curl, not in ChatGPT | Wrong path (Actions vs MCP) | Use directory `mcp.json` path above |
| Unknown tool `agentstack.agentstack.execute` | Host/model prefixed connector id onto `agentstack.execute` | Call the `tools/list` name `agentstack.execute` (or `agentstack_execute`). Server also accepts the double-prefix as an alias after Core hot-sync. |

## Live probe (operator)

```bash
python agentstack-core/scripts/mcp_connector_scan_live_test.py
```

Set `AGENTSTACK_API_KEY` or complete OAuth out-of-band for authenticated `tools/list`.

## Cross-links

- [DIRECTORY_SUBMIT.md](./DIRECTORY_SUBMIT.md)
- [docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md)
- [GPT_QUICKSTART.md](./GPT_QUICKSTART.md) (Actions path)
