# Create Payout with cURL

## What this example shows

This example creates a payout order with `POST /api/v2/digital/payouts` and signs the exact raw JSON string that is sent on the wire.

## PowerShell example

```powershell
$baseUrl = "https://your-vgpay-host.example.com"
$apiKey = "replace-with-your-api-key"
$secretKey = "replace-with-your-secret-key"

$timestamp = [DateTimeOffset]::UtcNow.ToUnixTimeMilliseconds().ToString()
$requestId = [guid]::NewGuid().ToString()
$body = '{"merchantOrderId":"TEST_OUT_2001","merchantUserId":"USER_001","amount":25.75,"currency":"USDT","network":"TRC20","withdrawAddress":"TFk7mJ2A2QYwW6ZrJ5u3pA8VdExample"}'
$payload = $timestamp + $requestId + $body

$hmac = [System.Security.Cryptography.HMACSHA256]::new([System.Text.Encoding]::UTF8.GetBytes($secretKey))
$signBytes = $hmac.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($payload))
$sign = [Convert]::ToBase64String($signBytes)
$hmac.Dispose()

curl.exe --request POST "$baseUrl/api/v2/digital/payouts" `
  --header "Content-Type: application/json" `
  --header "X-Api-Key: $apiKey" `
  --header "X-Api-Timestamp: $timestamp" `
  --header "X-Api-RequestId: $requestId" `
  --header "X-Api-Sign: $sign" `
  --data-raw $body
```

## Notes

- `withdrawAddress` is required for payout creation.
- Keep `$body` as one exact string from signing through `--data-raw`.
- Do not reuse payment request assumptions for payout handling.
