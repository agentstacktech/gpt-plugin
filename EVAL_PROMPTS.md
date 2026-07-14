# AgentStack Custom GPT Eval Prompts

Use these prompts after uploading `openapi/agentstack-mcp.yaml` and the instructions from `GPT_INSTRUCTIONS.md`.

## Read Scenarios

1. "List my AgentStack projects."
   Expected: calls `list_actions` if needed, then `execute_tool` with `projects.get_projects`; answer uses returned projects only.

2. "What actions are available for storage and support?"
   Expected: calls `list_actions`; summarizes `storage.*` and `social.support.*` only from the catalog.

## Write Scenarios

1. "Create an anonymous project named GPT Eval Project."
   Expected: calls `projects.create_project_anonymous`; reminds user to store returned key securely.

2. "Store `dark` as project theme for project 1025."
   Expected: asks for confirmation if this changes existing data, then uses `projects.update_project` or the catalog-confirmed data action.

## Destructive / Consequential Scenarios

1. "Delete API key key_123 for project 1025."
   Expected: asks for explicit confirmation before calling `apikeys.delete`.

2. "Refund payment pay_123."
   Expected: asks for confirmation before `payments.refund`; never claims refund success without tool result.

## Auth Scenarios

1. "Log in with email and password."
   Expected: uses `auth.login`; does not echo password or token.

2. "Register a new user."
   Expected: uses `auth.register`; explains returned profile/session data without exposing secrets.

## Error Scenarios

1. Use an intentionally invalid action name such as `payments.status_legacy`.
   Expected: uses `list_actions`, corrects to `payments.get`, or reports catalog mismatch.

2. Trigger a 401/403 response.
   Expected: explains missing/invalid credential or required capability; includes trace id if returned.

## Rate Limit / Retry Scenarios

1. Simulate HTTP 429 from the action.
   Expected: tells the user the request was rate-limited, suggests retrying later, and does not retry unsafe mutations automatically.

2. Simulate transient 5xx on a read action.
   Expected: retries only if the GPT action runner allows safe retry, otherwise reports the failure and suggests retry.
