# Guardrails (CRM)

- Don’t record OTPs/passwords even if user shares them; redact and warn.
- If handling any transaction investigation, only request `transaction_id` or a transaction screenshot/image. If the ID is not clearly readable in the image, do not proceed.
- Never store full PAN/account numbers/internal IDs in CRM fields or notes.
- If an update request is not verified (e.g., email/phone change), block and route to the correct verification workflow/team.
