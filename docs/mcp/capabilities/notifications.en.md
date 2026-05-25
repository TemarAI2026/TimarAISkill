# Notification Capability

## Scope

The phase-one MCP notification capability covers receiver-side handling of asynchronous order notifications. Its goal is not to invent a webhook contract, but to constrain how integrators and AI assistants implement a safe receiver around the published contract.

## Current phase-one expectations

- receive asynchronous notifications correctly
- implement idempotent handling
- reconcile notification data with local order records
- combine notification processing with order lookup and current status semantics when needed

## What callers should confirm first

Before implementing notification handling, confirm the currently published integration contract for:

- sender identity
- delivery path
- request headers
- signature or verification rule
- payload structure
- retry behavior
- success response expectation

If these items are not clearly published, do not guess them in the MCP layer.

## Receiver implementation notes

- log a stable request trace identifier
- preserve the raw request body and important headers for troubleshooting
- design idempotency around a stable event key or an order-plus-status transition
- verify order ID, amount, currency, network, and similar business fields against your own records before applying state changes
- treat notification payloads as final authority only when the published contract explicitly says so

## Common mistakes

- inventing webhook fields or signature rules that are not in the published contract
- processing duplicate notifications more than once
- updating local business state before reconciling notification values
- treating notification success as enough evidence when a follow-up order lookup is still required by business rules

## Exact references

- [`../../skill/domains/crypto/handle-webhook.en.md`](../../skill/domains/crypto/handle-webhook.en.md)
- [`../../skill/domains/shared/response-conventions.en.md`](../../skill/domains/shared/response-conventions.en.md)
- [`../../skill/domains/shared/error-handling.en.md`](../../skill/domains/shared/error-handling.en.md)
- [`../../skill/references/crypto/integration-checklist.en.md`](../../skill/references/crypto/integration-checklist.en.md)
