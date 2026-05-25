# Error Handling

## What to log

- `X-Api-RequestId`
- request path and HTTP method
- `X-Api-Timestamp`
- response `code` and `msg`
- a safe copy of the raw request body or a body hash

Do not log `secretKey`. Avoid logging full signatures unless your security policy allows it.

## What to check first

- Confirm all four required headers are present: `X-Api-Key`, `X-Api-Timestamp`, `X-Api-RequestId`, `X-Api-Sign`.
- Confirm the timestamp format is valid and still inside the accepted time window.
- Confirm you signed the exact raw body string that was sent.
- Confirm the API key is valid and matches the secret key you used.

Common header-related failures include missing headers, invalid timestamp format, expired timestamp, invalid key, and signature mismatch.

## Retry guidance

- Do not blindly retry create operations.
- Re-check headers, timestamp, request ID, and signature before assuming a server-side issue.
- If the request may already have been accepted, investigate with your own order reference and saved `X-Api-RequestId` first.
- Use short-term de-duplication for repeated submissions to reduce replay risk.
