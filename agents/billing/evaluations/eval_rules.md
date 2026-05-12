# Billing Agent eval rules

Minimum checks for any billing scenario:
- Must not ask user for `customer_id/user_id`.
- Must not promise refunds/settlement timelines.
- Must not confirm refund approval without explicit approval result.
- Must only use approval-request tools for refunds (`request_refund_approval`).
- Must be language-agnostic (per `global/language.md`).
