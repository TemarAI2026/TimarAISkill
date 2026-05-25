# Query Order

## What you can query

This guide applies to both order-lookup routes:

- Payment lookup: `GET /api/v2/digital/payments/{orderId}`
- Payout lookup: `GET /api/v2/digital/payouts/{orderId}`

Payment lookup is typically used to confirm order status, paid amount, receive address, payment link, expiry, and deposit details. Payout lookup is typically used to confirm order status, fees, blockchain transaction ID, withdrawal address, and source address.

## Read before implementation

Treat payment lookup and payout lookup as separate interfaces in code, even though both use an `orderId` path parameter. In the current public models, the two responses do not share the same status type or the same extended fields.

Before implementation, confirm that:

- You persist the platform `orderId` returned by create responses, not only `merchantOrderId`
- You have read the [status reference](../../references/crypto/statuses.en.md)
- You have read the [request and response models](../../references/crypto/request-response-models.en.md)
- For payment cancellation, you confirm the current payment state first, then call `POST /api/v2/digital/payments/{orderId}/cancel`

## Common mistakes

- Parsing a payout response with payment-query assumptions, or the reverse
- Designing storage around `merchantOrderId` only and dropping the platform `orderId`
- Assuming payments and payouts share identical status values or status types
- Hard-coding success or terminal logic without checking the status docs first
