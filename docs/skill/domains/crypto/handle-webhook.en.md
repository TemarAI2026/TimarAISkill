# Handle Webhook

## What to confirm first

Before implementing a webhook receiver, confirm that your public integration contract clearly defines:

- The actual webhook sender and trigger scenarios
- The request headers, signature, or verification rules
- The event payload shape and status fields
- Retry behavior, timeout expectations, and success-response expectations

If the current public docs do not define these points clearly, do not infer unsupported behavior by assumption. Re-check the latest public docs and your agreed integration contract first.

## Receiver guidance

Keep the receiver conservative, auditable, and replay-safe:

- Log request IDs. If the upstream sender does not provide a clear request ID, generate and log your own receiver trace ID and bind it to the raw request
- Retain the raw request body and important headers for troubleshooting and reconciliation
- Design idempotency around a stable event key or an order-plus-status transition so retries do not create duplicate business effects
- Treat webhook events as a final source of truth only when your own integration contract says that is valid
- Reconcile incoming order ID, amount, currency, network, and similar values against your own order records before updating business state
- Re-check the current public status docs, response conventions, and error-handling docs before production implementation

## Common mistakes

- Inventing webhook fields or verification rules when the public contract does not define them
- Failing to log request IDs or raw payloads, making retries and duplicates hard to investigate
- Skipping idempotency and processing duplicate notifications more than once
- Updating local order state immediately without comparing the notification to your own records and contract rules
