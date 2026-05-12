# Agentic Skills Registry

This directory defines a maintainable “skills spec layer” for domain agents (identity, policies, tools, permissions, workflows, escalation, memory, guardrails, retrieval).

This repo is **declarative only**:
- ✅ definitions, configs, instructions, policies, workflows
- ❌ runtime execution, API logic, infra, databases

**Loading order (recommended):**
1. Load `registry.yaml`
2. Load agent `agents/<agent>/agent.md` (identity)
3. Merge `global/*` (shared policies + tone)
4. Attach tools/permissions (`tools.yaml`, `permissions.yaml`)
5. Attach workflows/escalations (`workflows.yaml`, `escalation.yaml`)
6. Apply memory/retrieval/guardrails (`memory.yaml`, `retrieval.yaml`, `guardrails.md`)

Keep prompts modular and small. Put executable process logic in workflows, not in prompt text.

**File meanings (quick):**
- `agent.md`: core identity (role, objectives, tone, constraints)
- `tools.yaml`: tool names only (runtime resolves tool → MCP/action server)
- `permissions.yaml`: required capabilities (runtime enforces)
- `workflows.yaml`: workflow IDs the agent can run
- `memory.yaml`: memory access rules (scopes + masking)
- `guardrails.md`: forbidden behavior + safety/compliance rules
- `retrieval.yaml`: RAG sources + retrieval parameters
- `metadata.yaml`: ownership, tags, priority, channels
- `global/language.md`: language detection + response rules (merged into all agents)
- `global/guardrails.md`: cross-agent safety + privacy rules (merged into all agents)
- `global/tools.yaml`: optional global tool names (runtime resolves/implements)

**Spec packs (shared):**
- `contracts/`: tool request/response contracts (JSON Schema)
- `sla/`: SLA matrix for windows and escalation thresholds
- `taxonomy/`: complaint categories, severities, routing tags
- `definitions/`: shared condition definitions (what “pending_exceeds_sla_threshold” means, etc.)
- `i18n/`: canonical translation keys used by templates
