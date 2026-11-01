# ChatGPT and Codex directory submission

Package shape follows [Package your plugin](https://developers.openai.com/plugins/build/plugins) and [Upload and submit your plugin](https://developers.openai.com/plugins/deploy/submission).

Custom GPT Actions (`openapi/agentstack-mcp.yaml`) stay in this repo for the Actions path. The directory ZIP uses root `plugin.json`, `mcp.json`, and `skills/`.

`mcp.json` is a remote server, not a local process:

- `type` is `streamable-http` (Perplexity also accepts this value; do not switch it to `stdio`)
- `url` is `https://agentstack.tech/mcp`
- auth is OAuth dynamic client registration, with no API key and no fixed headers in the file

Perplexity skips local `stdio` servers, servers with no `type`, and servers that require fixed headers. A host that only offers a local connector will report that type as unavailable. Re-import this package so the connector is the remote URL above.

## Perplexity

1. Plugins → Add custom → Import plugin from GitHub: `https://github.com/agentstacktech/gpt-plugin` (or upload `agentstack-plugin.zip`).
2. Open the plugin and choose **Connect** on the AgentStack connector.
3. The connector URL is `https://agentstack.tech/mcp`, transport Streamable HTTP, authentication OAuth.
4. Complete AgentStack sign-in. The same connection is then available anywhere that connector is used.

## Build the ZIP

From the AgentStack workspace:

```bash
python provided_plugins/scripts/pack-gpt-plugin.py
```

The archive is `provided_plugins/gpt-plugin/.publish/agentstack-plugin.zip`. Files sit at the archive root. `.git` is omitted.

Check the package first:

```bash
node provided_plugins/scripts/audit-gpt-plugin.mjs
```

## Portal steps (founder)

1. Open ChatGPT Plugins as an organization owner (or a member with Apps Management Write) and complete developer verification.
2. Upload the ZIP. Resolve the copied findings, then upload again. Do not add `apps`, `.app.json`, hooks, or screenshots. This server has no custom UI, and the directory rejects screenshots unless a scan reports a UI template.
3. On MCPs, connect `https://agentstack.tech/mcp` and finish OAuth. Listing URLs must return their own pages, not the homepage shell: `public/privacy-policy/index.html`, `public/terms-of-service/index.html`, and `public/support/index.html` (nginx exact matches). Re-run the privacy assessment after that frontend build is live. Domain proof is already served at `https://agentstack.tech/.well-known/openai-apps-challenge` from `agentstack-frontend/public/.well-known/openai-apps-challenge` (exact token, no extra newline). If the portal shows a different token, replace that file and ship the frontend. Do not append a second token.
4. Rescan tools. The live list is one tool, `agentstack.execute`, with `readOnlyHint: false`, `destructiveHint: true`, and `openWorldHint: true`.
5. In Review details, add a test account and a demo video URL. The ZIP does not include credentials or `demo_recording_url`. Positive cases (5) and negative cases (3) are imported from `plugin.json`.
6. Submit for review. Publish only after approval.

`plugin.json` `name` is `agentstack`. Later uploads must keep that name and use a new `version` only when Lance orders a version bump.
