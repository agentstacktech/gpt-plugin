# Changelog

All notable changes to the AgentStack GPT (OpenAI) integration will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.26] - 2026-11-04

### Changed

- OpenAPI `info.version` and plugin.json **0.4.26** aligned with Core AgentGit release.

## [Unreleased]

### Added

- Agent Plugins package for ChatGPT, Codex, and other hosts such as Perplexity: root `plugin.json`, remote `mcp.json` (`https://agentstack.tech/mcp`, Streamable HTTP, OAuth DCR), getting-started skill, and square icons. No local stdio server. Custom GPT Actions schema is unchanged. See `DIRECTORY_SUBMIT.md`.
- `MCP_CONNECT.md` — directory MCP vs Custom GPT Actions, OAuth Connect checklist, troubleshooting.
- GPT instructions filter Cursor-only auth rows; `get-started` skill documents root `mcp.json`.

### Changed

- Regenerated `instructions/GPT_INSTRUCTIONS.md` with ChatGPT-specific auth overrides.
- `mcp.json` stays `streamable-http`. Type `http` makes ChatGPT count a second server and reject plugin-level `test_cases`. Do not add `.mcp.json`.
- Connector auth is `mixed` with OAuth discovery. The server already answers without a token; an `oauth`-only manifest left the ChatGPT connector uninstalled. The onboarding skill uses that bundled connector and does not ask for a second custom MCP server.

## [0.4.18] - 2026-09-08

### Changed

- Platform line **0.4.18**: OpenAPI `info.version`, `plugin.json`, and regenerated `instructions/GPT_INSTRUCTIONS.md` via `sync-gpt-instructions.mjs`.
- Capability routing aligned with live `GET /mcp/actions` catalog (568 public actions).

## [0.4.0] - 2026-02-23

### Changed

- Version aligned to global AgentStack 0.4.0 (OpenAPI info.version).

## [0.1.0] - 2026-02-23

### Added

- Initial release for ChatGPT (GPT Actions).
- OpenAPI 3.1 schema `openapi/agentstack-mcp.yaml`: single operation `execute_tool` (POST /mcp), API Key auth (X-API-Key).
- `GPT_INSTRUCTIONS.md`: Context, Instructions, and Additional notes for Custom GPT.
- `GPT_QUICKSTART.md`: get API key, create Custom GPT, add Action, paste instructions, example prompts.
- `README.md`: purpose, artifact list, Quick Start, links to MCP_CAPABILITY_MATRIX and plugin comparison.
- `ARTIFACTS.md`: artifact layout and backend (MCP URL, auth).
- `openapi/SCHEMA_VALIDATION.md`: GPT Actions compatibility checklist.
- `TESTING_AND_CAPABILITIES.md`: manual checklist, scenarios, link to full tool list.
- `docs/plugins/GPT_PLUGIN_POST_RELEASE_CHECKLIST.md` (in main repo): post-release and versioning.
