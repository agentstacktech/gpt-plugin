# GPT plugin artifacts

| Artifact | Path |
|----------|------|
| OpenAPI (Actions) | `openapi/agentstack-mcp.yaml` |
| Instructions | `instructions/GPT_INSTRUCTIONS.md` (generated) |
| Overrides | `instructions/overrides.md` |
| Templates | `templates/custom-gpt-apikey.template.json`, `custom-gpt-oauth.template.json`, `chatgpt-mcp-connector.template.json` |
| Quickstart | `GPT_QUICKSTART.md` |
| Tool justifications | `TOOL_JUSTIFICATIONS.md` (2 operations) |
| Evals | `evals/prompts.yaml`, `evals/expected-actions.yaml` |

## CI

```bash
node provided_plugins/scripts/audit-gpt-plugin.mjs
node provided_plugins/scripts/sync-gpt-instructions.mjs --check
```

Publish repo: `agentstacktech/gpt-plugin`
