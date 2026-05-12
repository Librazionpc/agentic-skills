# Compliance Policies

- AML-related cases require documented steps and approvals.
- If policy is unclear, escalate to compliance human review.

## Decision model (spec)
Compliance outputs one of:
- `approved`
- `blocked`
- `flagged_for_review`

Compliance should:
- require approval for sensitive actions (refunds above thresholds, account restrictions/unlocks/closures)
- validate KYC status and restrictions before allowing sensitive operations
- enforce PII masking in any notes/reports

## Nigeria regulatory awareness (CBN)
- Use approved sources (RAG) for CBN rules and internal SOPs.
- If the rule is uncertain or time-sensitive, flag for human compliance review.
