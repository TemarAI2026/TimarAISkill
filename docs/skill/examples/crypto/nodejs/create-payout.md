# Create Payout in Node.js

## What this example shows

This example sends `POST /api/v2/digital/payouts` and reuses one JSON string for both signing and HTTP transport.

## Example

```js
import crypto from "node:crypto";

const baseUrl = "https://your-vgpay-host.example.com";
const apiKey = "replace-with-your-api-key";
const secretKey = "replace-with-your-secret-key";

const requestBody = {
  merchantOrderId: "TEST_OUT_2001",
  merchantUserId: "USER_001",
  amount: 25.75,
  currency: "USDT",
  network: "TRC20",
  withdrawAddress: "TFk7mJ2A2QYwW6ZrJ5u3pA8VdExample"
};

const body = JSON.stringify(requestBody);
const timestamp = Date.now().toString();
const requestId = crypto.randomUUID();
const payload = `${timestamp}${requestId}${body}`;
const sign = crypto
  .createHmac("sha256", secretKey)
  .update(payload, "utf8")
  .digest("base64");

const response = await fetch(`${baseUrl}/api/v2/digital/payouts`, {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-Api-Key": apiKey,
    "X-Api-Timestamp": timestamp,
    "X-Api-RequestId": requestId,
    "X-Api-Sign": sign
  },
  body
});

const responseText = await response.text();
console.log(response.status);
console.log(responseText);
```

## Notes

- `withdrawAddress` is required for payout creation.
- Keep the `body` string unchanged after signing.
- Store the returned platform `orderId` for subsequent payout lookup.
