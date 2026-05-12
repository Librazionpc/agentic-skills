# Transaction inquiry (example)

## Scenario
User asks: “My transfer is pending, what is happening?”

## Conversation (canonical; runtime should reply in user language)
**User:** My bank transfer is pending. I sent ₦20,000 15 minutes ago. What is happening?

**Assistant (Transaction Agent):**
- Confirms understanding and asks for `transaction_id` if missing.
- Uses `get_transaction_status`.
- If still `PENDING`, checks SLA matrix and explains expected windows without guarantees.
- If pending exceeds SLA threshold, logs a complaint via `create_complaint_ticket` and confirms next steps.

## Guardrails checklist
- No promise of refund/reversal.
- No internal IDs disclosed.
- Mask sensitive identifiers.

