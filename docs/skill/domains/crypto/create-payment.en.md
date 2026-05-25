# Create Payment

## When to use this endpoint

Use `POST /api/v2/digital/payments` when you need to create a new incoming crypto payment order for a user. This endpoint fits flows where you need a payment link, a receive address, and expiry information.

A successful create response currently includes concepts such as `orderId`, `merchantOrderId`, `paymentUrl`, `amount`, `receiveAddress`, `currency`, `network`, and `expiresInSeconds`.

## Request fields to confirm

The current payment-create model includes:

- Required: `merchantOrderId`
- Required: `merchantUserId`
- Required: `amount`
- Required: `currency`
- Required: `network`
- Optional: `returnUrl`
- Optional: `cancelUrl`

Before you generate code, confirm:

- `merchantOrderId` is unique on your merchant side
- `merchantUserId` maps to your own user identifier
- `amount`, `currency`, and `network` come from valid business configuration
- `returnUrl` and `cancelUrl` are browser redirect destinations, not server-to-server notification fields

## Before code generation

Make sure your prompt or implementation notes include:

- The target endpoint is `POST /api/v2/digital/payments`
- The current request model does not include `callbackUrl`
- Your code must consume `orderId`, `paymentUrl`, `receiveAddress`, and `expiresInSeconds` from the create response
- Follow-up lookup belongs to [Query order](./query-order.en.md) and the [status reference](../../references/crypto/statuses.en.md)
- Signing, error handling, and response parsing should be implemented with the shared docs

Recommended companion reading:

- [Crypto endpoints](../../references/crypto/endpoints.en.md)
- [Request and response models](../../references/crypto/request-response-models.en.md)
- [Auth signing](../shared/auth-signing.en.md)
- [Response conventions](../shared/response-conventions.en.md)

## Common mistakes

- Adding `callbackUrl` to the current payment-create request
- Treating `merchantOrderId` as the same value as the platform `orderId`
- Omitting `network`, or collapsing network and currency into one field
- Using `returnUrl` or `cancelUrl` as webhook destinations
- Ignoring `expiresInSeconds` and continuing to present expired payment orders
