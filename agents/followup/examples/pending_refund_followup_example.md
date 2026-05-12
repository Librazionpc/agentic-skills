# Example: Pending refund follow-up

## Scenario
Billing agent submitted `request_refund_approval`, case remains pending.

## Expected behavior
- Use `ticket_status_check` and `workflow_state_query` to confirm current state.
- Send a customer-safe progress update (no guarantees).
- Schedule next check-in via `reminder_schedule_create`.
- If SLA breach occurs, escalate to Escalation Agent.

