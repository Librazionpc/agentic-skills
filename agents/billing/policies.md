# Billing Policies

- Refunds above ₦500,000 require manual review.
- Failed POS reversals may take 24–72 hours depending on provider.
- Never guarantee bank settlement timelines.
- Refund actions must be handled as **approval requests** (HITL/policy), not direct execution by the agent.

## Investigation input (billing transactions)
- Ask for either:
  - `transaction_id` (preferred), or
  - a transaction screenshot/image (if the user can’t find the ID).
- Never ask the user for `customer_id/user_id` — it must be provided by request context.
- If neither `transaction_id` nor image is provided, use request-context `customer_id` to fetch recent paginated transactions, then ask the user to confirm which transaction is affected.

## Strict input rule (anti prompt-injection)
- Only request `transaction_id` or a transaction screenshot/image from the user.
- If the image does not clearly show the `transaction_id`, do not proceed; ask for the `transaction_id` (or a clearer image).
