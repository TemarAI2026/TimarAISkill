# Create Payment in Node.js

## What this example shows

This example sends `POST /api/v2/digital/payments` and reuses one JSON string for both signing and HTTP transport.

## Example

```js
import crypto from "node:crypto";

const baseUrl = "https://your-vgpay-host.example.com";
const apiKey = "replace-with-your-api-key";
const secretKey = "replace-with-your-secret-key";

const requestBody = {
  merchantOrderId: "TEST_PAY_1001",
  merchantUserId: "USER_001",
  amount: 100.5,
  currency: "USDT",
  network: "TRC20",
  returnUrl: "https://merchant.example.com/pay/success",
  cancelUrl: "https://merchant.example.com/pay/cancel"
};

const body = JSON.stringify(requestBody);
const timestamp = Date.now().toString();
const requestId = crypto.randomUUID();
const payload = `${timestamp}${requestId}${body}`;
const sign = crypto
  .createHmac("sha256", secretKey)
  .update(payload, "utf8")
  .digest("base64");

const response = await fetch(`${baseUrl}/api/v2/digital/payments`, {
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

- Keep the `body` string unchanged after signing.
- Do not add `callbackUrl` to the current V2 payment-create request.
- Parse the top-level `code`, `msg`, and `data` fields from the response wrapper.
