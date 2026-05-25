# Query Order with cURL

## What this example shows

This example queries an existing order with no request body. For body-less requests, the signing payload is `timestamp + requestId`.

## PowerShell example

```powershell
$baseUrl = "https://your-vgpay-host.example.com"
$apiKey = "replace-with-your-api-key"
$secretKey = "replace-with-your-secret-key"
$orderId = "replace-with-platform-order-id"

$timestamp = [DateTimeOffset]::UtcNow.ToUnixTimeMilliseconds().ToString()
$requestId = [guid]::NewGuid().ToString()
$payload = $timestamp + $requestId

$hmac = [System.Security.Cryptography.HMACSHA256]::new([System.Text.Encoding]::UTF8.GetBytes($secretKey))
$signBytes = $hmac.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($payload))
$sign = [Convert]::ToBase64String($signBytes)
$hmac.Dispose()

curl.exe --request GET "$baseUrl/api/v2/digital/payments/$orderId" `
  --header "X-Api-Key: $apiKey" `
  --header "X-Api-Timestamp: $timestamp" `
  --header "X-Api-RequestId: $requestId" `
  --header "X-Api-Sign: $sign"
```

## Notes

- Use the platform `orderId` returned by the create response, not only `merchantOrderId`.
- To query a payout instead, change the path to `/api/v2/digital/payouts/{orderId}`.
- The payment query and payout query responses do not share the same full field set.
