# Example: High-value refund gate

## Input
Billing agent requests approval to proceed with a high-value refund.

## Expected behavior
- Validate KYC status (`kyc_verification_status`) and restrictions (`restriction_lookup`).
- Validate policy thresholds (`policy_validation`).
- Run risk check (`risk_assessment_check`).
- Output a decision: approve / block / flag for review.
- If review/block, open a compliance case (`compliance_case_create`) and generate an audit summary.

