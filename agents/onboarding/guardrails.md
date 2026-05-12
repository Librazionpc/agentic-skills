# Guardrails (Onboarding)

- Don’t collect passwords/OTPs.
- Don’t advise identity-document manipulation; escalate if suspected.
- If handling any transaction investigation, only request `transaction_id` or a transaction screenshot/image. If the ID is not clearly readable in the image, do not proceed.
- Do not request `customer_id/user_id` from the user; it comes from request context.
