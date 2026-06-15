# Common References

## Purpose

This page is the MCP-level shared fact entrypoint for all phase-one capabilities. Use it when you need the common runtime contract before going into capability-specific details.

## Shared runtime facts

Current shared runtime facts every integrator should confirm:

- required headers are `X-Api-Key`, `X-Api-Timestamp`, `X-Api-RequestId`, and `X-Api-Sign`
- with a request body, the signing payload is `timestamp + requestId + rawBody`
- without a request body, the signing payload is `timestamp + requestId`
- the signature formula is `Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`
- every request should use a fresh `X-Api-RequestId`

## Shared response facts

All current public responses use the same top-level wrapper:

- `code`
- `msg`
- `data`

Current success rule:

- `code == "0"` means success
- any other `code` means failure or further handling is required

Always persist or log:

- `X-Api-RequestId`
- returned `code`
- returned `msg`
- raw request and response when troubleshooting payment movement or signing issues

## Shared error facts

Common public error scenarios to check first:

- `400`: invalid parameters, missing headers, invalid timestamp, or expired timestamp
- `401`: signature mismatch
- `500`: server-side failure
- `40009`: duplicate `merchantOrderId`

Known current public message:

- `Timestamp expired.`

## MCP reference entrypoints

Use these MCP-facing reference pages first:

- endpoints: [`./endpoints.en.md`](./endpoints.en.md)
- request and response models: [`./request-response-models.en.md`](./request-response-models.en.md)
- status semantics: [`./status-semantics.en.md`](./status-semantics.en.md)
- error and retry guidance: [`./error-retry.en.md`](./error-retry.en.md)

Then use these exact lower-layer source references for final contract validation:

- headers: [`../../skill/references/shared/headers.en.md`](../../skill/references/shared/headers.en.md)
- signature examples: [`../../skill/references/shared/signature-examples.en.md`](../../skill/references/shared/signature-examples.en.md)
- error codes: [`../../skill/references/shared/error-codes.en.md`](../../skill/references/shared/error-codes.en.md)
- endpoints: [`../../skill/references/crypto/endpoints.en.md`](../../skill/references/crypto/endpoints.en.md)
- request and response models: [`../../skill/references/crypto/request-response-models.en.md`](../../skill/references/crypto/request-response-models.en.md)
- statuses: [`../../skill/references/crypto/statuses.en.md`](../../skill/references/crypto/statuses.en.md)
- integration checklist: [`../../skill/references/crypto/integration-checklist.en.md`](../../skill/references/crypto/integration-checklist.en.md)

## How to use this page

Recommended usage order:

1. confirm runtime headers and signing rules here
2. confirm response and error semantics here
3. open the target MCP capability page
4. use the MCP-facing reference pages above
5. validate final implementation details against the linked exact references

## Common mistakes

- using this summary page as the only source when generating final API client code
- forgetting that payment query status is a normalized string while payout query status is currently a raw `int`
- handling only business payload fields and ignoring wrapper fields such as `code` and `msg`
- keeping only merchant-side IDs and not preserving trace data such as `X-Api-RequestId`
