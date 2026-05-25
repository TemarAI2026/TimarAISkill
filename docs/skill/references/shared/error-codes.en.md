# Error Codes Reference

## Common Codes

| External code | Current meaning | Typical trigger | What to check next |
| --- | --- | --- | --- |
| `"0"` | Success | The request completed successfully. | Continue with normal response handling. |
| `400` | `ParameterInvalid` | Missing headers, invalid timestamp format, expired timestamp, or request validation failure. | Check required fields, all four headers, and whether the message includes `Timestamp expired.`. |
| `401` | `SignaturesNotMatch` | Signature mismatch. | Recheck `secretKey`, the raw body string, `timestamp`, `requestId`, and the signing formula. |
| `500` | `ServerError` | Server-side failure or no more specific public code. | Log `X-Api-RequestId`, retain the raw request, and escalate with support if needed. |
| `40001` | `CoinNotExist` | The currency is unknown or not enabled in the current configuration. | Verify `currency` against your enabled business configuration. |
| `40007` | `OrderNotFound` | The payment order was not found. | Check the platform `orderId` and confirm the order belongs to the current merchant. |
| `40008` | `WithdrawalNotFound` | The payout order was not found. | Check the platform `orderId` and confirm you are querying a payout order. |
| `40009` | `OutOrderNoIsExists` | The merchant order number already exists. | Verify whether `merchantOrderId` was submitted before. |
| `40012` | `WithdrawalHasBeenTurnedOff` | Payouts are currently disabled. | Confirm the account configuration and payout enablement status. |
| `40021` | `BusinessFrozenOrInactive` | The business is frozen or inactive. | Check merchant status and contact support. |

## Notes

- In current Crypto V2 responses, `code` is a string and success is always returned as `"0"`.
- The same code can appear with different public messages; `Timestamp expired.` is a known public message today.
- For troubleshooting, always retain `X-Api-RequestId`, the raw request body, and the returned `code`/`msg`.
