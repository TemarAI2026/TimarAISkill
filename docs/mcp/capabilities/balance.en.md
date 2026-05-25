# Balance Capability

## Scope

The phase-one MCP balance capability currently covers account balance lookup for published crypto balances.

## Current endpoint family

- `GET /api/v2/digital/balances`

## What callers should prepare

Before calling the balance capability:

- runtime environment, `apiKey`, `secretKey`, and signing behavior must already be configured
- the caller should already know why balance is being checked, for example admin display, payout pre-check, or reconciliation
- downstream code should distinguish account-view data from order-state data

## Response concepts

The current public balance response returns one or more balance items with these concepts:

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

`availableBalance` is what remains usable for new operations. `lockedBalance` is already reserved or frozen. `totalBalance` is the sum of both.

## Consumption notes

- consume balances per `currency`
- do not use `totalBalance` where only `availableBalance` is valid
- do not treat one balance read as the only concurrency control for payouts
- persist or log enough balance snapshots for troubleshooting and reconciliation if balances drive money movement decisions

## Common mistakes

- reading `totalBalance` as if it were fully spendable
- ignoring `lockedBalance`
- using balance lookup as a substitute for business-side reservation logic
- mixing balance data with order query response handling

## Exact references

- [`../../skill/domains/crypto/query-balance.en.md`](../../skill/domains/crypto/query-balance.en.md)
- [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
- [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)
- [`../../skill/references/crypto/integration-checklist.en.md`](../../skill/references/crypto/integration-checklist.en.md)
