# Contracts (Spec-only)

This folder defines **tool contracts** (inputs/outputs/errors) as declarative specs.

- Tool names must match the strings in `agents/*/tools.yaml`.
- Runtime/MCP implementations live outside this repo, but should conform to these contracts.

Layout:
- `contracts/tools/<tool>.json` — tool contract (JSON Schema for request/response)
- `contracts/errors.yaml` — shared error codes (optional)

