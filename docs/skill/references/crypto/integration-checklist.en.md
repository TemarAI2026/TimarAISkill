# Crypto Integration Checklist

## Before Development

- Confirm `X-Api-Key` and `secretKey` are stored securely in the environment, and never log `secretKey`.
- Implement all four required headers and signing based on the exact raw request body string.
- Generate a fresh `X-Api-RequestId` for every request and apply short-term de-duplication.
- Generate a unique `merchantOrderId` for every create request.
- Persist `merchantOrderId`, the returned platform `orderId`, `X-Api-RequestId`, the request timestamp, and the raw request/response.
- Model payments and payouts as separate flows, and do not mix `returnUrl`, `cancelUrl`, and `withdrawAddress`.
- Design status normalization from the current statuses reference, with a safe fallback for unknown future values.
- Plan balance checks and failure handling in your business layer before enabling payouts.

## Before Go-Live

- Verify that all 6 public endpoint paths and methods match the current configuration exactly.
- Test signatures for both requests with bodies and requests without bodies, covering Unix milliseconds and ISO8601 timestamps.
- Verify logging and alerting for missing headers, signature mismatch, `Timestamp expired.`, order not found, and duplicate merchant order scenarios.
- Verify that payment creation persists `paymentUrl`, `receiveAddress`, `expiresInSeconds`, and the platform `orderId`.
- Verify that the payment query and cancel flow checks the order first and then calls the cancel endpoint by `orderId`.
- Verify that payout creation persists `fee` and `withdrawAddress`, and that payout queries update `totalFee`, `networkFee`, `serviceFee`, `txId`, and `sourceAddress`.
- Verify that balance handling clearly distinguishes `availableBalance`, `lockedBalance`, and `totalBalance`.
- Verify that retry logic does not blindly resubmit create requests and can investigate with the saved `X-Api-RequestId` and `orderId`.
