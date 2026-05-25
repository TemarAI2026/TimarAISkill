# Create Payment with cURL

## What this example shows

This example creates a payment order with `POST /api/v2/digital/payments` and signs the exact raw JSON string that is sent on the wire.

## PowerShell example

```powershell
$baseUrl = "https://your-vgpay-host.example.com"
$apiKey = "replace-with-your-api-key"
$secretKey = "replace-with-your-secret-key"

$timestamp = [DateTimeOffset]::UtcNow.ToUnixTimeMilliseconds().ToString()
$requestId = [guid]::NewGuid().ToString()
$body = '{"merchantOrderId":"TEST_PAY_1001","merchantUserId":"USER_001","amount":100.50,"currency":"USDT","network":"TRC20","returnUrl":"https://merchant.example.com/pay/success","cancelUrl":"https://merchant.example.com/pay/cancel"}'
$payload = $timestamp + $requestId + $body

$hmac = [System.Security.Cryptography.HMACSHA256]::new([System.Text.Encoding]::UTF8.GetBytes($secretKey))
$signBytes = $hmac.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($payload))
$sign = [Convert]::ToBase64String($signBytes)
$hmac.Dispose()

curl.exe --request POST "$baseUrl/api/v2/digital/payments" `
  --header "Content-Type: application/json" `
  --header "X-Api-Key: $apiKey" `
  --header "X-Api-Timestamp: $timestamp" `
  --header "X-Api-RequestId: $requestId" `
  --header "X-Api-Sign: $sign" `
  --data-raw $body
```

## Notes

- `merchantOrderId`, `merchantUserId`, `amount`, `currency`, and `network` are required.
- `returnUrl` and `cancelUrl` are optional browser redirect fields.
- Do not add `callbackUrl` to the current V2 payment-create request.
- Keep `$body` as one exact string from signing through `--data-raw`.
