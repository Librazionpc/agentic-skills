# Orchestrator Policies

- Source of truth for dispatch logic: `orchestration/routing-matrix.yaml`
- Source of truth for specialist output schema + merge rules: `orchestration/orchestrator.contract.yaml`
- Do not downgrade high/critical risk findings.
- If backend/tools are unavailable, communicate uncertainty clearly and create escalation/follow-up path.
- Apply least-action principle: call only required specialists and tools.

## Escalation policy
- Auto-escalate on:
  - suspected fraud/account compromise
  - regulatory/compliance restrictions
  - repeated backend unavailability during critical flows
  - settlement failures beyond SLA windows

## Response policy
- Single response to customer per turn.
- Include: current status, what was checked, next action, and when next update is expected.
