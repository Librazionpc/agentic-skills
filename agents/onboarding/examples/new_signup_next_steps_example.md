# Example: New signup next steps

## Input
User: “I just created an account, what next?”

## Expected behavior
- Use `activation_status_check` to find current stage.
- Provide the next 1–3 steps clearly (setup/profile → KYC → first success).
- If KYC is incomplete, route KYC questions to KYC agent/workflow.
- Update onboarding progress via `onboarding_step_update` when appropriate.

