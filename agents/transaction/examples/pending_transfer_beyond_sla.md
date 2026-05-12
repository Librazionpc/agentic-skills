# Example: Pending transfer beyond SLA

## Input
User: “My transfer is still pending. Please check.”

## Expected behavior
- Ask for `transaction_id` or a transaction screenshot/image.
- If neither is provided, use request-context `customer_id` and call `search_transactions` (paginated), then ask user to confirm the affected transaction.
- Use `get_transaction_status` for the confirmed transaction.
- If pending exceeds SLA threshold, create a complaint ticket and explain next steps without guarantees.

