# Signature Examples

## With Body

```text
timestamp = 1716200000123
requestId = req-20260525-0001
rawBody = {"merchantOrderId":"M123","merchantUserId":"U123","amount":10.5,"currency":"USDT","network":"TRC20"}
payload = 1716200000123req-20260525-0001{"merchantOrderId":"M123","merchantUserId":"U123","amount":10.5,"currency":"USDT","network":"TRC20"}
```

## Without Body

```text
timestamp = 2026-05-25T10:30:45Z
requestId = req-20260525-0002
payload = 2026-05-25T10:30:45Zreq-20260525-0002
```

## Example Signing Formula

```text
signature = Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))
```
