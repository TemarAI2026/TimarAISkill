# Create Payout in C#

## What this example shows

This example sends `POST /api/v2/digital/payouts` and reuses one serialized JSON string for both signing and HTTP transport.

## Example

```csharp
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Text.Json;

var baseUrl = "https://your-vgpay-host.example.com";
var apiKey = "replace-with-your-api-key";
var secretKey = "replace-with-your-secret-key";

var request = new
{
    merchantOrderId = "TEST_OUT_2001",
    merchantUserId = "USER_001",
    amount = 25.75m,
    currency = "USDT",
    network = "TRC20",
    withdrawAddress = "TFk7mJ2A2QYwW6ZrJ5u3pA8VdExample"
};

var body = JsonSerializer.Serialize(request);
var timestamp = DateTimeOffset.UtcNow.ToUnixTimeMilliseconds().ToString();
var requestId = Guid.NewGuid().ToString();
var payload = timestamp + requestId + body;

using var hmac = new HMACSHA256(Encoding.UTF8.GetBytes(secretKey));
var sign = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(payload)));

using var client = new HttpClient();
using var content = new StringContent(body, Encoding.UTF8, "application/json");
content.Headers.ContentType = new MediaTypeHeaderValue("application/json");

using var message = new HttpRequestMessage(HttpMethod.Post, $"{baseUrl}/api/v2/digital/payouts")
{
    Content = content
};
message.Headers.Add("X-Api-Key", apiKey);
message.Headers.Add("X-Api-Timestamp", timestamp);
message.Headers.Add("X-Api-RequestId", requestId);
message.Headers.Add("X-Api-Sign", sign);

using var response = await client.SendAsync(message);
var responseText = await response.Content.ReadAsStringAsync();

Console.WriteLine(response.StatusCode);
Console.WriteLine(responseText);
```

## Notes

- `withdrawAddress` is required for payout creation.
- Keep the `body` string unchanged after signing.
- Store the returned platform `orderId` for subsequent payout lookup.
