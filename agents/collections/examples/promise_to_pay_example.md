# Example: Promise to pay

## Input
User: “I will pay next week.”

## Expected behavior
- Use `debt_status_check` to confirm what is due and when.
- Offer approved options (promise-to-pay or reschedule) without negotiation outside policy.
- Record the plan via `repayment_schedule_update` (as a request, policy enforced at runtime).
- Send a confirmation reminder using `payment_reminder_send`.
- Update CRM status using `crm_update`.

