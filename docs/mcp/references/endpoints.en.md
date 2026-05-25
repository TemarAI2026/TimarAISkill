# Endpoint References

## Purpose

This page lists the currently published MCP-facing endpoint family for the phase-one crypto capability set.

## Current published endpoints

| Method | Path | Capability | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/v2/digital/payments` | `payment` | Create a payment order. |
| `GET` | `/api/v2/digital/payments/{orderId}` | `payment` | Query a payment order. |
| `POST` | `/api/v2/digital/payments/{orderId}/cancel` | `payment` | Cancel a payment order. |
| `POST` | `/api/v2/digital/payouts` | `payout` | Create a payout order. |
| `GET` | `/api/v2/digital/payouts/{orderId}` | `payout` | Query a payout order. |
| `GET` | `/api/v2/digital/balances` | `balance` | Query account balances. |

## Usage notes

- payment and payout do not share the same create endpoint
- order lookup currently uses the platform `orderId` path parameter
- cancel is currently defined only for payment orders
- balance lookup is an account-view endpoint, not an order endpoint

## Common mistakes

- calling the payment path with payout request fields
- querying by `merchantOrderId` when the published path requires platform `orderId`
- assuming payout has a public cancel endpoint because payment has one
- treating balance lookup as if it returns order status

## Exact source reference

- [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
