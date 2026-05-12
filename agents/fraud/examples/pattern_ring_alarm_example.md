# Example: Coordinated ring alarm

## Scenario
Runtime detects similar transfers across multiple accounts (shared device cluster).

## Expected behavior
- Use `pattern_monitor_query` to confirm pattern type and risk level.
- If high/critical, raise an internal alarm via `fraud_alarm_raise`.
- Create/attach a fraud case (`fraud_case_create`) and escalate to Escalation/Compliance as needed.
