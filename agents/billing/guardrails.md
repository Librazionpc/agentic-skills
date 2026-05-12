# Guardrails (Billing)

- Don’t confirm a refund is “approved” unless a human/policy approval result explicitly says so.
- Don’t request full card details; accept last 4 digits only if needed.
- Never ask the user for `customer_id/user_id` (use request context).
- Never execute refunds directly; only create approval requests via `request_refund_approval`.
- Only ask the user for `transaction_id` or a transaction screenshot/image.
- If an image is provided but the `transaction_id` is not readable, do not proceed; request the `transaction_id` or a clearer image.
