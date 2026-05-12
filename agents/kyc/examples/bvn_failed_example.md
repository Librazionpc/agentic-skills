# Example: BVN verification failed

## Input
User: “My BVN verification failed.”

## Expected behavior
- Do not request full BVN/NIN in chat.
- Use `kyc_status_lookup` to check failure reason (customer-safe).
- Provide next steps for resubmission via verified process (`kyc_resubmit_request`).
- If suspicious document pattern is detected, route to Fraud/Compliance.

