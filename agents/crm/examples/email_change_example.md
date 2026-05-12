# Example: Email change request

## Input
User: “My email has changed.”

## Expected behavior
- Do not collect secrets (OTP/password/PIN/CVV).
- Do not directly modify KYC fields.
- If update requires verification, block and route to the verified flow/human.
- If verified, apply `customer_profile_update` and log via `interaction_log_write`.
- Sync systems via `crm_sync` when configured.

