# Example: Suspicious transfer burst

## Input
User: “Multiple ₦200k transfers happened in 2 minutes. I didn’t do this.”

## Expected behavior
- Treat as high risk; do not ask for secrets.
- Trigger risk checks (`risk_score_calculate`, `device_fingerprint_check`, `behavior_analysis_query`).
- Request containment actions via policy-controlled tools (`account_freeze` / `account_security_actions`) if allowed.
- Create a fraud case (`fraud_case_create`) and escalate to humans as needed.
- Provide customer safety guidance without disclosing internal signals.

## Pattern monitoring add-on
- If the pattern looks coordinated across accounts, query `pattern_monitor_query` and raise an internal alarm via `fraud_alarm_raise`.
