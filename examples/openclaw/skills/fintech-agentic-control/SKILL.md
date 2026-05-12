---
name: fintech-agentic-control
description: Orchestrate the Librazionpc/agentic-skills fintech spec pack with OpenClaw by selecting the right agent profile, loading workflows, and mapping declarative tool names to real MCP tools before execution. Use when handling fintech support operations (KYC, fraud, transactions, billing, collections, escalation, CRM follow-up) with policy-driven responses.
---

# Fintech Agentic Control

Use this skill as a runtime adapter for `workspace/agentic-skills`.

## Runtime contract

1. Pick the agent profile from `agentic-skills/registry.yaml`.
2. Load in this order:
   - `agents/<agent>/agent.md`
   - `global/*.md`
   - `agents/<agent>/guardrails.md` and `policies.md`
   - `agents/<agent>/workflows.yaml`
   - `agents/<agent>/tools.yaml`
   - `agents/<agent>/permissions.yaml`
3. Resolve every tool name in `tools.yaml` to a real MCP/tool endpoint before acting.
4. If any required tool is unmapped, stop and request mapping.
5. Execute only allowed workflow + permission combinations.

## Tool mapping rule

- Declarative names (example: `transaction_lookup`) are not executable by themselves.
- Use `references/mcp-tool-map.template.yaml` as the source of truth.
- Refuse sensitive actions if the mapped tool is missing permission or policy coverage.

## Minimum safety gates

- Enforce PII redaction and masking policies before outbound text.
- Never fabricate transaction/kyc/fraud outcomes when backend lookup fails.
- Escalate immediately on fraud/compliance/regulatory hold triggers.

## MCP onboarding checklist

When MCP tools are provided:
1. Fill `references/mcp-tool-map.template.yaml`.
2. Verify each mapped tool with a safe read-only probe.
3. Run one dry-run workflow (recommend: `faq_answer_workflow` or `transaction_inquiry_workflow`).
4. Promote to live handling only after successful dry-run.
