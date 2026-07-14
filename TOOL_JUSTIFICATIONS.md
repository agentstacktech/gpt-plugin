# Tool justifications for OpenAI app approval

Use the text below when OpenAI asks for "Tool justification" for each tool. Copy the **Justification** block for the matching tool into the form.

Annotations meaning:
- **readOnlyHint true**: Tool only reads data; no create/update/delete.
- **openWorldHint false**: Tool calls our API only (AgentStack backend), not arbitrary external services.
- **destructiveHint true**: Tool performs irreversible deletion or equivalent.
- **idempotentHint true**: Same arguments produce the same outcome; safe to retry.

---

## projects_get_projects

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=true

**Justification:**  
This tool lists projects (optionally filtered). It only reads data from our backend; it does not create, update, or delete anything. It always targets the AgentStack API. Calling it multiple times with the same parameters returns the same list, so it is idempotent.

---

## projects_get_project

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=true

**Justification:**  
This tool fetches a single project by ID. It only reads data; it has no side effects. It calls only our API. Same project_id always returns the same project data, so it is idempotent.

---

## projects_create_project_anonymous

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=false, idempotentHint=false

**Justification:**  
This tool creates a new project without requiring user login. It mutates state (creates a project and returns API keys). It does not delete or irreversibly destroy data. It is not idempotent: each call creates a new project.

---

## projects_get_stats

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=true

**Justification:**  
This tool returns statistics for a project (e.g. usage). It only reads data; no create/update/delete. It calls only our API. Same inputs yield the same stats, so it is idempotent.

---

## projects_create_project

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=false, idempotentHint=false

**Justification:**  
This tool creates a new project (typically with authentication). It mutates state. It does not perform irreversible deletion. It is not idempotent: each call creates a new project.

---

## projects_update_project

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=false, idempotentHint=false

**Justification:**  
This tool updates an existing project's metadata (e.g. name). It mutates state but does not delete the project or data irreversibly. It is not idempotent in the strict sense (multiple updates can change state in sequence).

---

## projects_delete_project

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=true, idempotentHint=false

**Justification:**  
This tool deletes a project and can remove associated data. The operation is irreversible: once deleted, the project and its data cannot be recovered through this API. It is not idempotent (first call deletes; subsequent calls may return an error).

---

## auth_get_profile

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=true

**Justification:**  
This tool returns the current user's profile (when authenticated). It only reads profile data; no mutations. It calls only our API. Same auth context yields the same profile, so it is idempotent.

---

## auth_login

**Annotations:** readOnlyHint=true, openWorldHint=false, destructiveHint=false, idempotentHint=false

**Justification:**  
This tool authenticates a user (e.g. email/password) and returns a session or token. It does not create or delete user data; it only verifies credentials and establishes a session. It calls only our API. It is not idempotent: each call may extend or refresh session state.

---

## auth_register

**Annotations:** readOnlyHint=false, openWorldHint=false, destructiveHint=false, idempotentHint=false

**Justification:**  
This tool registers a new user account. It mutates state (creates a user). It does not perform irreversible deletion. It is not idempotent: duplicate registration attempts may fail or create distinct records depending on backend rules.
