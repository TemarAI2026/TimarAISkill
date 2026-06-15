# Authentication and Signing

## Scope

This page defines the MCP runtime contract for request authentication in phase one. It is the minimum rule set an integrator or AI coding assistant must follow before calling any published API capability.

## Required runtime inputs

Prepare these inputs for every request:

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

Do not reuse a previous `requestId`, timestamp, or signature across requests.

## Required headers

Current public calls require all four headers:

- `X-Api-Key`
- `X-Api-Timestamp`
- `X-Api-RequestId`
- `X-Api-Sign`

`X-Api-Timestamp` currently accepts Unix milliseconds or ISO8601. A known public failure mode is `Timestamp expired.`, so keep client time aligned with server time and avoid stale retries.

## Signing payload rules

- With a request body: `timestamp + requestId + rawBody`
- Without a request body: `timestamp + requestId`

`rawBody` means the exact string sent on the wire. Sign the final serialized payload, not a reconstructed object.

## Signing formula

```text
Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))
```

## Runtime checklist

Before sending a request:

1. Generate a fresh `requestId`.
2. Produce the exact outbound body string, if the request has a body.
3. Build the signing payload from the real timestamp, request ID, and raw body rule above.
4. Generate `X-Api-Sign`.
5. Send the request without reformatting the body afterward.

## Do not guess

- Do not add the HTTP method, path, query string, separators, or line breaks unless a published contract explicitly says so.
- Do not replace an empty body with `{}` or any other placeholder.
- Do not sign one JSON string and send another.
- Do not log `secretKey`.
- Do not assume timestamp tolerance beyond the currently documented behavior.

## Exact references

Use these pages when implementing or reviewing signing code:

- [`../../skill/references/shared/headers.en.md`](../../skill/references/shared/headers.en.md)
- [`../../skill/references/shared/signature-examples.en.md`](../../skill/references/shared/signature-examples.en.md)
- [`../../skill/domains/shared/auth-signing.en.md`](../../skill/domains/shared/auth-signing.en.md)
