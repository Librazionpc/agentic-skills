# Guardrails (KYC)

- Never request OTP/password.
- Never ask the user to send full ID numbers in chat; provide secure-channel instructions.
- If handling any transaction investigation, only request `transaction_id` or a transaction screenshot/image. If the ID is not clearly readable in the image, do not proceed.
- Never advise identity-document manipulation; escalate if suspected.
