# Create Payout

## When to use this endpoint

Use `POST /api/v2/digital/payouts` when you need to send funds from the platform to an external crypto wallet address. This is not just the inverse of the payment flow; it is a separate outbound flow with a stronger focus on destination address accuracy, fees, and post-submission tracking.

A successful create response currently includes concepts such as `orderId`, `merchantOrderId`, `merchantUserId`, `amount`, `fee`, `currency`, `network`, and `withdrawAddress`.

## Request fields to confirm

The current payout-create model includes:

- Required: `merchantOrderId`
- Required: `merchantUserId`
- Required: `amount`
- Required: `currency`
- Required: `network`
- Required: `withdrawAddress`

Before you generate code, confirm:

- `withdrawAddress` is on the correct network and matches `network`
- `merchantOrderId` is unique on your merchant side
- `merchantUserId` meets your audit or user-association needs
- `amount` and balance checks are handled in your own business layer before submission

## Before code generation

Make sure your prompt or implementation notes include:

- The target endpoint is `POST /api/v2/digital/payouts`
- The current request model does not include `callbackUrl`
- Your code must consume `orderId`, `fee`, and `withdrawAddress` from the create response
- Follow-up lookup belongs to [Query order](./query-order.en.md) and the [status reference](../../references/crypto/statuses.en.md)
- Payout and payment are different flows and should not share payment-only fields such as `returnUrl` or `cancelUrl`

Recommended companion reading:

- [Crypto endpoints](../../references/crypto/endpoints.en.md)
- [Request and response models](../../references/crypto/request-response-models.en.md)
- [Query balance](./query-balance.en.md)
- [Error handling](../shared/error-handling.en.md)

## Common mistakes

- Omitting `withdrawAddress`, or passing an address that does not match `network`
- Treating payment and payout as the same workflow with different statuses
- Adding `callbackUrl` to the current payout-create request
- Reusing payment-only fields such as `returnUrl` or `cancelUrl`
- Saving only `merchantOrderId` and not the platform `orderId` needed for subsequent lookup
