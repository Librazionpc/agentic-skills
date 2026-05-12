# Example: Duplicate debit

## Input
User: “I was debited twice for the same bill payment. Please check.”

## Expected behavior
- Ask for `transaction_id` or a transaction screenshot/image.
- If neither is provided, use request-context `customer_id` and call `search_transactions` (paginated), then ask user to confirm which transaction(s) are affected.
- Use `get_transaction_status` to verify statuses.
- Create a refund request for approval via `request_refund_approval` if policy allows; never promise outcome.

