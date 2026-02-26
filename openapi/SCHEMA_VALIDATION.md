# OpenAPI schema — GPT Actions compatibility

**Schema:** `agentstack-mcp.yaml`  
**Checked against:** [Getting started with GPT Actions](https://developers.openai.com/api/docs/actions/getting-started)

| Requirement | Status |
|-------------|--------|
| OpenAPI 3.1.0 | Yes — `openapi: 3.1.0` |
| `info.title`, `info.description`, `info.version` | Yes — ChatGPT uses description for action relevance |
| Operation `summary` and `description` | Yes — `execute_tool` has both; model can decide when to call |
| Request body with clear parameter descriptions | Yes — `tool` (string), `params` (object) with descriptions and examples |
| Authentication (API Key or OAuth) | Yes — `ApiKeyAuth` in header `X-API-Key`; `security` on operation |
| Responses (200 and errors) | Yes — 200, 400, 401, 404 described |

Schema is compatible with GPT Actions. When adding the Action in Custom GPT, use Authentication type "API Key", header name `X-API-Key`.
