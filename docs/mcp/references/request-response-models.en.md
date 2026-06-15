# Request and Response Models

## Purpose

This page summarizes the current public request and response model shapes that matter most when implementing MCP-facing payment, payout, and balance clients.

## Shared top-level response wrapper

All current public responses use the same top-level wrapper:

| Field | Meaning |
| --- | --- |
| `code` | Business result code. Success is `"0"`. |
| `msg` | Human-readable result or error message. |
| `data` | Business payload whose shape depends on the endpoint. |

## Payment create

Current request fields:

- required: `merchantOrderId`, `merchantUserId`, `amount`, `currency`, `network`
- optional: `returnUrl`, `cancelUrl`

Current response fields to retain:

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## Payment query

Current response fields commonly consumed:

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

## Payout create

Current request fields:

- required: `merchantOrderId`, `merchantUserId`, `amount`, `currency`, `network`, `withdrawAddress`

Current response fields to retain:

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## Payout query

Current response fields commonly consumed:

- `orderId`
- `merchantOrderId`
- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

## Balance query

Current response item fields:

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

## Common mistakes

- ignoring the wrapper fields `code` and `msg`
- inventing `callbackUrl` in create requests where it is not currently published
- keeping only `merchantOrderId` and discarding platform `orderId`
- treating balance fields as if they were order fields

## Exact source reference

- [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)
