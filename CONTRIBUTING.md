# Contributing to `agentic-skills`

## Rules
- This repo is **spec-only**. Do not add runtime code, database access, or MCP implementations.
- Keep tools declarative: `agents/*/tools.yaml` are **names only**.
- Financial actions must be **approval requests** (HITL) unless explicitly allowed by policy and enforced at runtime.
- Templates must be **language-agnostic** (canonical + `t_*` keys), see `global/language.md` and `i18n/keys.yaml`.

## Adding a tool
1. Add tool name to the appropriate agent’s `tools.yaml`.
2. Add a contract file in `contracts/tools/<tool>.json`.
3. Add any new shared error codes to `contracts/errors.yaml` (if needed).

### MCP implementation checklist (outside this repo)
When implementing a tool in `agentic-mcp`:
- Enforce the request schema from `contracts/tools/<tool>.json`.
- Mask/redact sensitive fields (PAN, account numbers, internal IDs, secrets).
- Return stable error codes (match `contracts/errors.yaml`).
- Add audit logging metadata (customer_id from context, ticket_id if available).
- Do not expose direct REST endpoints to agents; agents only call MCP tools.

## Adding a workflow
1. Add workflow id to agent `workflows.yaml`.
2. Add a workflow file under `workflows/<workflow>.yaml`.

## Validation (recommended)
- YAML syntax: all `*.yaml` should parse cleanly.
- Structural schemas (optional): see `schemas/agent_workflows.schema.json`, `schemas/agent_tools.schema.json`, `schemas/agent_permissions.schema.json`.

## Adding templates
- Prefer canonical templates in `templates/` with `t_*` keys.
- Add keys to `i18n/keys.yaml`.
