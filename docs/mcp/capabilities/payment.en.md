# Payment Capability

## Scope

The phase-one MCP payment capability currently covers incoming crypto payment order operations. The published operation set is:

- create payment
- query payment order
- cancel payment order

## Current endpoint family

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`

## What callers should prepare

Before calling the payment capability:

- merchant onboarding and account configuration must already be complete
- runtime environment, `apiKey`, `secretKey`, and signing behavior must already be configured
- your business system must generate a unique `merchantOrderId`
- your business system must know the intended `merchantUserId`, `amount`, `currency`, and `network`

This MCP layer assumes merchant identity and upstream business context are already prepared.

## Create operation

Use `POST /api/v2/digital/payments` to create a new payment order.

Current request concepts:

- Required: `merchantOrderId`
- Required: `merchantUserId`
- Required: `amount`
- Required: `currency`
- Required: `network`
- Optional: `returnUrl`
- Optional: `cancelUrl`

Current create response concepts that callers should persist or consume:

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## Query and cancel operations

Use `GET /api/v2/digital/payments/{orderId}` to retrieve the current order state.

The current query response exposes concepts such as:

- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

Use `POST /api/v2/digital/payments/{orderId}/cancel` only for the platform `orderId` you intend to cancel. Do not assume `merchantOrderId` can be used in place of `orderId`.

## Status consumption notes

The current payment query status is exposed on the wire as normalized strings such as:

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

Persist the raw response and handle unknown future status values safely.

## Common mistakes

- adding `callbackUrl` to the current create-payment request
- treating `merchantOrderId` and platform `orderId` as the same identifier
- mixing `currency` and `network` into one field
- using `returnUrl` or `cancelUrl` as server-to-server notification fields
- ignoring `expiresInSeconds` and continuing to present an expired payment order

## Exact references

- [`../../skill/domains/crypto/overview.en.md`](../../skill/domains/crypto/overview.en.md)
- [`../../skill/domains/crypto/create-payment.en.md`](../../skill/domains/crypto/create-payment.en.md)
- [`../../skill/domains/crypto/query-order.en.md`](../../skill/domains/crypto/query-order.en.md)
- [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
- [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)
- [`../../skill/references/crypto/statuses.en.md`](../../skill/references/crypto/statuses.en.md)
