# Example: Refund timeline FAQ

## Input
User: “How long does a refund take?”

## Expected behavior
- Use `rag_search` / `policy_retrieval` to retrieve official refund timelines.
- Answer with documented ranges only (no guarantees).
- If user reports a specific failed transaction, route to Billing/Transaction rather than guessing.

