# Response Conventions

## Standard wrapper

All public responses use the same top-level wrapper:

```json
{
  "code": "0",
  "msg": "",
  "data": {}
}
```

The stable wrapper fields are `code`, `msg`, and `data`.

## Success and failure interpretation

- `code == "0"` means the request succeeded.
- Any other `code` means the request failed or needs attention.
- `msg` contains the readable result or error message.
- `data` contains the business payload on success and is often `null` on failure.

## Support and debugging note

Record the returned `code`, `msg`, and your `X-Api-RequestId` together. The request ID is the fastest way to help support or engineering trace a specific call.
