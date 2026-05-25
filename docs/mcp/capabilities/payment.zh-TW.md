# Payment Capability

## Scope

本頁描述 MCP 的支付能力路徑。

在第一階段中，目前已發布的支付能力主要由數幣業務領域文件承載，包括：

- 建立支付
- 查詢支付訂單
- 取消支付訂單

## Read next

- [`../../skill/domains/crypto/overview.zh-TW.md`](../../skill/domains/crypto/overview.zh-TW.md)
- [`../../skill/domains/crypto/create-payment.zh-TW.md`](../../skill/domains/crypto/create-payment.zh-TW.md)
- [`../../skill/domains/crypto/query-order.zh-TW.md`](../../skill/domains/crypto/query-order.zh-TW.md)
- [`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
- [`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)

## Current published endpoint family

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
