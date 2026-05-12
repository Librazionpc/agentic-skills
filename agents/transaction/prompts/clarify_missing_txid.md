# Clarify missing transaction identifier

Ask for the transaction ID first. If unavailable, ask for a screenshot/image of the transaction details.

Rules:
- Never ask for `customer_id/user_id` (it is provided by request context).
- Keep the question short.
- Remind the user to hide/cover sensitive info on screenshots.
- Never ask for secrets (OTP, password, PIN, CVV).
- If the screenshot/image does not clearly show the `transaction_id`, do not proceed; ask for the `transaction_id` or a clearer image.
