# Base Repo Spec (What to create next)

This file tells you exactly what additional files/content to add so it matches the specs already implemented in this repo.

## 1) RAG source IDs (you must provide)

Update RAG source IDs in each agent’s `retrieval.yaml` to match your knowledge system.

Example for Transaction agent:
- File: `agents/transaction/retrieval.yaml`
- Replace sample IDs like `redtech_support_kb_v1`, `redtech_sla_v1` with your real IDs.

Example for Billing agent:
- File: `agents/billing/retrieval.yaml`
- Replace sample IDs like `refunds_policy`, `dispute_resolution`, `card_billing` with your real IDs.

What you must provide (list of IDs):
- `refunds_policy` equivalent (refund eligibility + timelines + reversals)
- `dispute_resolution` equivalent (dispute/chargeback SOP + evidence requirements)
- `card_billing` equivalent (fees/charges + card/POS reversal guidance)
- `redtech_support_kb_v1` equivalent (general support KB)
- `redtech_sla_v1` equivalent (SLA windows and escalation thresholds)

## 0) What you still must build outside this repo (checklist)

This repo is complete as a **spec layer**. To go live, you still need:

### A) RAG / Knowledge repo
- Create the knowledge sources and assign stable source IDs.
- Ensure each ID used in `agents/*/retrieval.yaml` exists and is permissioned correctly.
- Add/maintain a “source-id registry” in your RAG repo so IDs don’t drift.
 - Optional automation: use `agentic-rag/skills_mapping.yaml` + `agentic-rag/pipelines/sync_skills.py` to auto-write `agents/*/retrieval.yaml`.

### B) MCP tools repo (`agentic-mcp`)
- Implement every tool name referenced by agents (see `agents/*/tools.yaml`).
- Validate inputs/outputs against `contracts/tools/*.json`.
- Return stable error codes aligned with `contracts/errors.yaml`.
- Mask/redact sensitive fields (PAN/account numbers/internal IDs/secrets) before returning data to agents.
- Add audit metadata (customer_id from context, ticket_id when present).
 - Implement strict allowlists for any web-access tool (e.g., `regulatory_web_check`).

### C) Runtime / Orchestration repo (`openclaw-runtime`)
- Enforce permissions/capabilities from `agents/*/permissions.yaml`.
- Enforce HITL approvals for sensitive actions (any `request_*_approval` tools).
- Enforce the strict anti prompt-injection rule:
  - only request `transaction_id` or transaction image from user,
  - if image does not clearly show ID, stop and request again.
- Parse `sla/sla_matrix.yaml` durations and compute flags like `pending_exceeds_sla_threshold`.
- Provide thresholds as config (e.g., high-value amount), not hardcoded in prompts.
- Provide language detection + template translation (see `global/language.md`, `i18n/keys.yaml`).
- Enforce allowlists/caching/audit for `regulatory_web_check` (if enabled).
- Add replayable request logs for operational troubleshooting (inputs, tool calls, decisions) with PII redaction.

### D) Fee schedule / charge label source (Billing)
- Provide a canonical fee mapping (RAG doc or structured catalog) so `fee_breakdown` explanations are correct.

## Always check before release (don’t forget)
- All `agents/*/retrieval.yaml` source IDs exist in RAG.
- All tools implemented in MCP match `contracts/tools/*.json`.
- All sensitive actions are approval-gated in runtime (HITL) and audited.
- SLA parsing from `sla/sla_matrix.yaml` is implemented and tested.

## Later tasks (keep tracked)
- Replace placeholder RAG source IDs in all `agents/*/retrieval.yaml` with your production IDs.
- Set runtime config values for thresholds referenced by conditions:
  - `high_value_transaction` amount
  - `high_outstanding_amount` amount
  - `repeated_missed_payments` counts/windows
- Decide how to handle condition aliases (example: `account_takeover_suspected` vs `suspected_account_takeover`) in runtime normalization.

## 2) Tool implementation mapping (lives outside this repo)

This repo defines contracts only.

You must implement tool names in:
- `agentic-mcp` (tool servers), and/or
- `openclaw-runtime` (orchestration + enforcement)

### MCP vs REST (don’t mix this up)
- In this repo, agents call **tool names** only (e.g., `get_transaction_status`).
- In production, those tool calls should go to **MCP tool servers** (`agentic-mcp`).
- MCP servers may internally call **REST**/gRPC services (transactions, complaints, loans, CRM), but agents must never call REST endpoints directly.

Practical rule:
- **Skills repo**: declares tools + contracts
- **MCP repo**: implements tools (may wrap REST)
- **Runtime**: enforces permissions, rate limits, audit logs, HITL approvals, masking/redaction

### REST service guidance (for your MCP implementers)
When you build tools, each tool should map to one or more backend REST endpoints with:
- input validation (schema)
- output masking/redaction
- stable error codes (match `contracts/errors.yaml`)
- audit logging metadata (who/why, ticket id if present)

Tool names that must exist (Complaint intake + investigation helpers):
- `get_transaction_status`
- `search_transactions`
- `get_user_account_summary`
- `fetch_payment_trace`
- `create_complaint_ticket`
- `append_complaint_note`
- `classify_complaint`
- `suggest_resolution`

Tool names that must exist (Transaction agent — money movement truth layer):
- `transaction_lookup` (legacy alias supported; see contract)
- `transaction_status_check`
- `transaction_trace`
- `settlement_status_query`
- `bank_confirmation_check`
- `ledger_reconciliation_check`
- `search_transactions`

Refund approval (Billing):
- `request_refund_approval`

Billing investigation/explanation (Billing):
- `payment_status_check`
- `ledger_trace`
- `fee_breakdown`

Tool contracts are here:
- `contracts/tools/*.json`

## 2.1 Tool contract inputs you must support

At minimum, your runtime/MCP tools should accept these inputs (per contracts):
- `transaction_id` for all transaction/billing lookups
- `customer_id` from request context for `search_transactions` (paginated)
- complaint payloads for `create_complaint_ticket` / `append_complaint_note`

Important:
- Agents must never ask the user for `customer_id/user_id` — runtime passes it in request context.
- For transaction investigation, agents only request `transaction_id` or a transaction image. If the image does not clearly show the ID, the agent must stop and ask again (anti prompt-injection).

### Regulatory web checks (Nigeria/CBN)
If you want “current CBN rules” beyond RAG snapshots, implement this optional tool:
- `regulatory_web_check` (contract: `contracts/tools/regulatory_web_check.json`)

Implementation requirements (MCP/runtime):
- Enforce a strict domain allowlist (e.g., `cbn.gov.ng` and any approved regulators).
- Cache results and log queries (audit).
- Return summaries suitable for internal compliance review (not customer-facing legal advice).

## 2.2 Tool naming conventions (recommended)
- Read-only lookups: `get_*`, `search_*`, `*_check`, `*_trace`
- Controlled writes: `create_*`, `append_*`, `*_send`, `*_update`
- Approval-only money actions: `request_*_approval` (never `refund_*` or `reverse_*`)

## 9) Remaining gaps in this repo (tool contracts)

All agent tool names now have matching contracts in `contracts/tools/`.

If you later rename tools or change payloads:
- update the agent `tools.yaml`
- update the corresponding contract JSON in `contracts/tools/`
- keep error codes aligned with `contracts/errors.yaml`

Note:
- `transaction_lookup` is treated as a legacy alias for `get_transaction_status` in its contract.

## 1.1 Retrieval source ID inventory (replace with your production IDs)

These are the current `sources:` IDs used across `agents/*/retrieval.yaml`. In your RAG/knowledge repo you must either:
- create these IDs exactly, or
- replace them with your production IDs consistently.

Current IDs:
- `account_activation`
- `account_security`
- `aml_guidelines`
- `bank_error_codes`
- `card_billing`
- `cbn_regulations`
- `collections_policy`
- `compliance_sop`
- `crm_note_templates`
- `crm_taxonomy`
- `dispute_resolution`
- `document_guidelines`
- `escalation_playbook`
- `faq_canonical`
- `fintech_policies`
- `followup_templates`
- `fraud_triage`
- `hardship_process`
- `help_center`
- `issue_taxonomy`
- `kyc_requirements`
- `legal_threat_process`
- `nigeria_fintech_policy`
- `onboarding_requirements`
- `outage_comms_guidelines`
- `redtech_sla_v1`
- `redtech_support_kb_v1`
- `refunds_policy`
- `regulatory_playbooks`
- `repayment_templates`
- `scam_playbooks`
- `settlement_windows`
- `signup_troubleshooting`
- `sla_policies`
- `sops_public`
- `ticketing_sop`
- `transfer_statuses`
- `verification_failures`

## 3) Complaint taxonomy (you can extend)

Edit:
- `taxonomy/complaints.yaml`

Add your production categories (IDs), required fields, defaults, and routing.

## 4) SLA matrix (you can extend)

Edit:
- `sla/sla_matrix.yaml`

Add/adjust rules for any new service types, providers, and escalation thresholds.

Implementation note:
- `sla/sla_matrix.yaml` uses human-readable durations (e.g., `5m`, `30s`, `30m–24h`).
- Your runtime should parse these into machine durations and compute flags like `pending_exceeds_sla_threshold`.

## 5) Thresholds and “computed conditions” (runtime config)

This repo defines what conditions *mean*:
- `definitions/escalation_conditions.yaml`

Runtime must define numeric thresholds and compute flags, for example:
- what amount qualifies as `high_value_transaction`
- how to compute `pending_exceeds_sla_threshold` from `sla/sla_matrix.yaml`

### What you must provide (threshold policy)
- `billing_high_value_amount_ngn`: default currently implied by Billing escalation as `> 500000`
- Any additional tiers you want, e.g.:
  - `medium_value_amount_ngn`
  - `vip_customer_threshold` (optional)
  - `max_auto_actions` (optional; even for approval requests)

Where to apply:
- `agents/billing/escalation.yaml` (amount triggers)
- `definitions/escalation_conditions.yaml` (meaning stays here; numbers live in runtime config)

## 6) i18n keys for canonical templates (optional but recommended)

Canonical templates use keys like `{{t_acknowledge}}`.

Edit:
- `i18n/keys.yaml`

Add any new `t_*` keys used by templates, with an English default.

## 7) Add new agents (checklist)

When you add a new agent directory under `agents/<agent_id>/`, include:
- `agent.md`
- `policies.md`
- `tools.yaml` (names only)
- `permissions.yaml` (capabilities only)
- `workflows.yaml` (IDs only)
- `escalation.yaml`
- `memory.yaml`
- `guardrails.md`
- `retrieval.yaml`
- `metadata.yaml`

Then register it in:
- `registry.yaml`
- `routing.yaml` (if it’s routable by issue type)

## 7.1 Transaction vs Billing separation (architecture rule)

Keep these roles clean to avoid compliance and safety risk:

- **Transaction Agent** = “Where is my money?” (money movement state)
  - Focus: transaction lifecycle status, traces, settlement windows, SLA checks
  - Must NOT: decide refunds/fees/disputes

- **Billing Agent** = “Why did you take my money / can I get it back?” (money correctness)
  - Focus: charges/fees explanations, disputes, refund eligibility, approval requests
  - Must NOT: execute money movement or expose ledger internals

Routing rule of thumb:
- movement/state questions → `transaction`
- correctness/refund/fee/dispute questions → `billing`

## 8) Fee schedule and charge labels (Billing needs this)

To make `fee_breakdown` explanations consistent (e.g., “why ₦50?”), you must provide a canonical fee mapping in your billing/knowledge systems.

What you must provide (one of these approaches):
- **RAG approach (recommended):** a billing/fees document indexed in RAG (and referenced by Billing `retrieval.yaml`) that defines:
  - fee label → plain-language explanation
  - when it applies (conditions)
  - examples customers understand
  - any regulatory wording requirements

AND/OR
- **Structured mapping approach:** a structured fee catalog in your core systems that `fee_breakdown` can reference and return as customer-safe labels.

Minimum required fields for fee items returned by `fee_breakdown`:
- `label`
- `amount`
- `currency`
