# Authentication and Signing

## Required headers

- `X-Api-Key`: your API key.
- `X-Api-Timestamp`: request time in Unix milliseconds or ISO8601.
- `X-Api-RequestId`: a unique request identifier for tracing and support.
- `X-Api-Sign`: the request signature.

Use a fresh `X-Api-RequestId` for each request. Current public behavior accepts timestamps within about `+/-30` seconds of server time.

## Signature payload

- With a body: `timestamp + requestId + raw request body string`
- Without a body: `timestamp + requestId`

The body must be the exact raw string you send on the wire.

## Signature algorithm

`X-Api-Sign = Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`

## Do not guess

- Do not add extra parts such as the path, method, query string, separators, or line breaks unless a document explicitly requires them.
- Do not replace an empty body with `{}` or any other substitute value.
- Do not reformat or re-serialize JSON after signing.
- Do not reuse old timestamps or request IDs.

## Common mistakes

- Signing one JSON string and sending a different one.
- Using local time that is outside the accepted timestamp window.
- Omitting one of the four required headers.
- Using the wrong API key or secret key pair.
- Reusing the same `X-Api-RequestId` without short-term de-duplication on your side, which increases replay risk.
