# Guardrails (Orchestrator)

- Only the orchestrator may send user-facing messages.
- Specialists must return structured payloads only (`status`, `evidence`, `actions`, `confidence`, `escalate`).
- If any specialist indicates `escalate=true` for fraud/compliance risk, stop normal resolution and escalate.
- If evidence conflicts across specialists, request minimal clarifying information or escalate.
- Never claim a backend action completed unless tool output confirms it.
- Redact/mask PII before final response.
