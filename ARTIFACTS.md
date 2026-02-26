# GPT Plugin AgentStack — Format of artifacts

**Version:** 0.1  
**Date:** 2026-02-23  
**Purpose:** Single source of truth for where each artifact lives in this repo (Elegant Minimalism, Decomposition).

---

## Artifact layout

| Artifact | Location | Description |
|----------|----------|-------------|
| **OpenAPI 3.1 schema** | `openapi/agentstack-mcp.yaml` | Schema for GPT Actions: one operation (execute MCP tool) + API Key auth. Version in `info.version`. |
| **Custom GPT instructions** | `GPT_INSTRUCTIONS.md` | Context, Instructions, Additional notes — copy into ChatGPT Custom GPT builder. |
| **Quick Start** | `GPT_QUICKSTART.md` | Steps: get API key → create Custom GPT → add Action → auth → paste instructions → example prompts. |
| **Overview** | `README.md` | What this integration is, what’s included, links to MCP_SERVER_CAPABILITIES and plugin docs. |
| **Testing** | `TESTING_AND_CAPABILITIES.md` | Manual checklist, scenarios, link to full tool list. |
| **Changelog** | `CHANGELOG.md` | Keep a Changelog format; version on schema/instructions changes. |
| **Post-release** | `docs/plugins/GPT_PLUGIN_POST_RELEASE_CHECKLIST.md` (in main repo) | Post-release checklist; do not duplicate in this folder. |

---

## Backend

- **MCP endpoint:** `https://agentstack.tech/mcp` (same as Cursor/Claude plugins).
- **Auth:** API Key in header `X-API-Key`. User gets key via anonymous project or account (see GPT_QUICKSTART).

---

## Regenerating the OpenAPI schema (optional)

The schema can be generated from the live MCP tool registry (single source of truth). From the **agentstack-core** directory:

```bash
cd agentstack-core && python -m mcp.generate_openapi_gpt --output ../provided_plugins/gpt-plugin/openapi/agentstack-mcp.yaml
```

Requires PyYAML (`pip install pyyaml`). The generated schema includes the current list of tool names from `MCP_TOOLS_REGISTRY` in the API description.

---

**Genetic code (reference):** `gpt_plugin_agentstack.artifacts.gen1`
