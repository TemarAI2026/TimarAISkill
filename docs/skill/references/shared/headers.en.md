# Headers Reference

## Header Table

| Header | Required | Description |
| --- | --- | --- |
| `X-Api-Key` | Yes | Merchant API key. |
| `X-Api-Timestamp` | Yes | Request time in Unix milliseconds or ISO8601. |
| `X-Api-RequestId` | Yes | Unique request identifier for tracing, troubleshooting, and short-term de-duplication. |
| `X-Api-Sign` | Yes | Base64 signature produced by the current signing rule. |

## Notes

- Generate a fresh `X-Api-RequestId` for every request.
- With a request body, the signing payload is `timestamp + requestId + rawBody`.
- Without a request body, the signing payload is `timestamp + requestId`.
- A known public error message is `Timestamp expired.`, which usually means the timestamp format or time window should be checked first.
- Public guidance recommends short-term de-duplication on `X-Api-RequestId` to reduce replay risk.
