# cURL Examples

## Purpose

These examples show the exact request shape, headers, and signing flow for the published crypto endpoints. Each sample preserves the same raw JSON string for both signing and transport.

## Environment values to replace

- `baseUrl`: your VGPAY API host
- `apiKey`: your assigned API key
- `secretKey`: your assigned signing secret
- `merchantOrderId`, `merchantUserId`, `orderId`, and `withdrawAddress`: your own business values

## Example files

- [Create payment](./create-payment.md)
- [Create payout](./create-payout.md)
- [Query order](./query-order.md)

## Important rule

Do not rebuild or reformat the JSON after calculating `X-Api-Sign`. The `body` string you sign must be the same string sent by `curl.exe`.
