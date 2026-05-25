# Error and Retry Guidance

## Purpose

This page summarizes the current public error-handling and retry boundaries for MCP-facing integrations.

## What to log

Always retain:

- `X-Api-RequestId`
- request path and method
- `X-Api-Timestamp`
- response `code`
- response `msg`
- a safe copy of the raw body or a body hash

Do not log `secretKey`.

## What to check first

Before escalating an error, check:

1. all four required headers are present
2. timestamp format and freshness
3. the exact raw body string used for signing
4. API key and secret key pairing
5. whether the request may already have been accepted

## Common public error cases

- `400`: invalid parameters, missing headers, invalid timestamp, or expired timestamp
- `401`: signature mismatch
- `500`: server-side failure
- `40007`: payment order not found
- `40008`: payout order not found
- `40009`: duplicate `merchantOrderId`

Known current public message:

- `Timestamp expired.`

## Retry rules

- do not blindly retry create-payment or create-payout operations
- investigate with your own order reference and saved `X-Api-RequestId` first
- use short-term de-duplication on repeated submissions
- only retry after confirming the failure is not caused by signing, headers, or duplicate business identifiers

## Common mistakes

- retrying create requests as if they were always safe and idempotent
- troubleshooting without `X-Api-RequestId`
- logging secrets instead of trace data
- treating `500` as proof that no downstream side effect happened

## Exact source references

- [`../../skill/domains/shared/error-handling.en.md`](../../skill/domains/shared/error-handling.en.md)
- [`../../skill/references/shared/error-codes.en.md`](../../skill/references/shared/error-codes.en.md)
