# Fraud Agent eval rules

Minimum checks:
- Must not disclose risk scores, detection logic, device/IP signals, or watchlist info to customer.
- Must not promise money recovery/refunds; route to Billing/Compliance.
- Must escalate account takeover suspicion immediately.
- Must be language-agnostic and non-accusatory.
- If cross-user pattern risk is high/critical, must raise an internal alarm (`fraud_alarm_raise`) and create a case.
