# AgentStack MCP — ChatGPT / Codex / Perplexity

**Gene:** `repo.plugins.bundle.gen1` · Spec: [Agent Plugins 1.0](https://agent-plugins.org/) · Backend: `core.mcp.tools.gen1`

## What “provides MCP” means here

| Integration | Files | What the user sees |
|-------------|-------|-------------------|
| **Directory plugin** | Root `plugin.json` + `mcp.json` + `skills/` + `.codex-plugin/plugin.json` | Portable hosts discover MCP from root `mcp.json` only (one server, `type` + `url`). Codex reads `.codex-plugin/plugin.json` with `mcpServers: "./mcp.json"`. Do not ship `.mcp.json` or top-level `mcpServers` in `plugin.json` — duplicates confuse Codex and portal upload. Skill icons: `skills/get-started/agents/openai.yaml` + `skills/get-started/assets/icon.png`. |
| **Custom GPT Actions** | `openapi/agentstack-mcp.yaml` | REST `list_actions` / `execute_tool` — **not** the same UI as directory MCP |

If you only created a Custom GPT with Actions and skipped the directory ZIP, the host may show **no MCP connector** — that is expected. Either import the directory package or stay on the Actions path ([GPT_QUICKSTART.md](./GPT_QUICKSTART.md)).

## Directory package checklist

1. Build: `python provided_plugins/scripts/pack-gpt-plugin.py` → `provided_plugins/gpt-plugin/.publish/agentstack-plugin.zip`
2. ZIP root must include **`mcp.json`** (streamable-http, `https://agentstack.tech/mcp`)
3. Upload ZIP in the ChatGPT Plugins portal (org owner + developer verification)
4. On **MCPs** tab: connect server → OAuth sign-in to AgentStack
5. Rescan tools — expect one tool: `agentstack.execute`

## Auth

`mcp.json` does not embed OAuth settings. The Agent Plugins schema allows only `type` and `url` on the server; an `extensions` block makes ChatGPT ignore the server, so the plugin lists MCP and the MCP page stays empty. The live server advertises `noauth` plus `oauth2` on `agentstack.execute` and publishes discovery at:

- `https://agentstack.tech/mcp/.well-known/oauth-protected-resource`
- `https://agentstack.tech/mcp/.well-known/oauth-authorization-server`

The server answers `initialize` and `tools/list` without a token. Sign-in is the plugin connector's **Connect** button, not a second custom MCP server.

No API key in `mcp.json`. No fixed `Authorization` header.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|----------------|-----|
| No MCP / “local only” | Old ZIP without `mcp.json`, or Actions-only setup | Rebuild ZIP; import directory package |
| `test_cases require exactly one MCP server` | `mcp.json` type is `http`, or both `mcp.json` and `.mcp.json` exist | One `mcp.json`, type `streamable-http`, no `.mcp.json` |
| Plugin lists MCP, page has no AgentStack server | Codex failed to parse `mcp.json` (`extensions` on server) or duplicate manifests | Server keys only `type` and `url`; restart Codex after ZIP install |
| Newspaper / shield icon after install | Skill lacks `agents/openai.yaml` icons | Ship `skills/get-started/agents/openai.yaml` and `assets/icon.png` |
| Empty tools list | Session not scoped | Retry Connect; probe `tools/list` via [mcp_connector_scan_live_test.py](../../agentstack-core/scripts/mcp_connector_scan_live_test.py) |
| Works in curl, not in ChatGPT | Wrong path (Actions vs MCP) | Use directory `mcp.json` path above |

## Live probe (operator)

```bash
python agentstack-core/scripts/mcp_connector_scan_live_test.py
```

Set `AGENTSTACK_API_KEY` or complete OAuth out-of-band for authenticated `tools/list`.

## Cross-links

- [DIRECTORY_SUBMIT.md](./DIRECTORY_SUBMIT.md)
- [docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md](../../docs/plugins/MCP_CHATGPT_GEMINI_GUIDE_RU.md)
- [MCP_QUICKSTART.md](./MCP_QUICKSTART.md) (Actions path)
