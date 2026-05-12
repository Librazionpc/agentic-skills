# Runtime Checklist

- Validate YAML syntax for `agentic-skills` files.
- Confirm selected workflow exists in `workflows/*.yaml`.
- Confirm agent has permission required for requested action.
- Confirm every referenced tool has mapping in `mcp-tool-map.template.yaml`.
- Run one read-only probe against each mapped backend tool.
- Log escalation reason when guardrails force handoff.
