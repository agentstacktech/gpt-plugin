# Changelog

All notable changes to the AgentStack GPT (OpenAI) integration will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.0] - 2026-02-23

### Changed

- Version aligned to global AgentStack 0.4.0 (OpenAPI info.version).

## [0.1.0] - 2026-02-23

### Added

- Initial release for ChatGPT (GPT Actions).
- OpenAPI 3.1 schema `openapi/agentstack-mcp.yaml`: single operation `execute_tool` (POST /mcp), API Key auth (X-API-Key).
- `GPT_INSTRUCTIONS.md`: Context, Instructions, and Additional notes for Custom GPT.
- `GPT_QUICKSTART.md`: get API key, create Custom GPT, add Action, paste instructions, example prompts.
- `README.md`: purpose, artifact list, Quick Start, links to MCP_SERVER_CAPABILITIES and plugin comparison.
- `ARTIFACTS.md`: artifact layout and backend (MCP URL, auth).
- `openapi/SCHEMA_VALIDATION.md`: GPT Actions compatibility checklist.
- `TESTING_AND_CAPABILITIES.md`: manual checklist, scenarios, link to full tool list.
- `docs/plugins/GPT_PLUGIN_POST_RELEASE_CHECKLIST.md` (in main repo): post-release and versioning.
