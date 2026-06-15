# Payout Capability

## Scope

The phase-one MCP payout capability currently covers outbound crypto payout order operations. The published operation set is:

- create payout
- query payout order

## Current endpoint family

- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`

## What callers should prepare

Before calling the payout capability:

- merchant onboarding and account configuration must already be complete
- runtime environment, `apiKey`, `secretKey`, and signing behavior must already be configured
- your business system must generate a unique `merchantOrderId`
- your business system must know the intended `merchantUserId`, `amount`, `currency`, `network`, and `withdrawAddress`
- your own business layer should already decide how it checks available funds and submission permissions

This MCP layer does not replace your own approval, risk, or reservation logic.

## Create operation

Use `POST /api/v2/digital/payouts` to create a new payout order.

Current request concepts:

- Required: `merchantOrderId`
- Required: `merchantUserId`
- Required: `amount`
- Required: `currency`
- Required: `network`
- Required: `withdrawAddress`

Current create response concepts that callers should persist or consume:

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## Query operation

Use `GET /api/v2/digital/payouts/{orderId}` to retrieve the current payout state.

The current query response exposes concepts such as:

- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

Persist both the platform `orderId` and your own `merchantOrderId` so subsequent lookup and reconciliation can succeed.

## Status follow-up notes

Current payout query status is returned on the wire as a raw `int` value. Map it with the published payout status table and handle future unknown values safely.

Do not assume payout finality only from submission success. Downstream business state normally needs follow-up polling or notification reconciliation.

## Common mistakes

- omitting `withdrawAddress`, or using an address that does not match `network`
- treating payout as the same workflow as payment
- adding payment-only fields such as `returnUrl`, `cancelUrl`, or `callbackUrl`
- storing only `merchantOrderId` and discarding the platform `orderId`
- treating the first create response as the final completion state

## Exact references

- [`../../skill/domains/crypto/overview.en.md`](../../skill/domains/crypto/overview.en.md)
- [`../../skill/domains/crypto/create-payout.en.md`](../../skill/domains/crypto/create-payout.en.md)
- [`../../skill/domains/crypto/query-order.en.md`](../../skill/domains/crypto/query-order.en.md)
- [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
- [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)
- [`../../skill/references/crypto/statuses.en.md`](../../skill/references/crypto/statuses.en.md)
