# OpenAPI schema validation

**SoT:** `agentstack-core/mcp/generate_openapi_gpt.py`

Regenerate:

```bash
export PYTHONPATH="$PWD:$PWD/agentstack-core"   # bash
python -m mcp.generate_openapi_gpt -o provided_plugins/gpt-plugin/openapi/agentstack-mcp.yaml
python -m mcp.generate_openapi_gpt -o docs/api/agentstack-mcp-openapi.json --format json
```

Check:

```bash
python -m mcp.generate_openapi_gpt -o provided_plugins/gpt-plugin/openapi/agentstack-mcp.yaml --check
node provided_plugins/scripts/validate-gpt-openapi.mjs
```

## Required paths

| Path | operationId | Notes |
|------|-------------|-------|
| `POST /mcp` | `execute_tool` | `x-openai-isConsequential: true` |
| `GET /mcp/actions` | `list_actions` | `x-openai-isConsequential: false` |

## Security

- `ApiKeyAuth` (`X-API-Key`)
- `OAuth2Auth` (ecosystem authorize/token URLs)

Action counts in `info.description` must come from the live registry — no hard-coded `60+` strings.
