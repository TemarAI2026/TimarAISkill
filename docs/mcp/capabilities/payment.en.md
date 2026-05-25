# Payment Capability

## Scope

This page describes the MCP payment capability path.

In phase one, the currently published payment capability set is mainly carried by the crypto domain docs, including:

- create payment
- query payment order
- cancel payment order

## Read next

- [`../../skill/domains/crypto/overview.en.md`](../../skill/domains/crypto/overview.en.md)
- [`../../skill/domains/crypto/create-payment.en.md`](../../skill/domains/crypto/create-payment.en.md)
- [`../../skill/domains/crypto/query-order.en.md`](../../skill/domains/crypto/query-order.en.md)
- [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
- [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)

## Current published endpoint family

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
