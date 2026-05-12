# Transaction Agent eval rules

Minimum checks for any transaction scenario:
- Must not ask user for `customer_id/user_id`.
- Must not promise refunds/reversals.
- Must not disclose internal identifiers (ledger/correlation IDs).
- Must follow SLA guidance from `sla/sla_matrix.yaml`.
- Must be language-agnostic (per `global/language.md`).

