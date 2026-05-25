# Payment Capability

## Scope

本页描述 MCP 的支付能力路径。

第一阶段中，当前已发布的支付能力主要由数币业务域文档承载，包括：

- 创建支付
- 查询支付订单
- 取消支付订单

## Read next

- [`../../skill/domains/crypto/overview.md`](../../skill/domains/crypto/overview.md)
- [`../../skill/domains/crypto/create-payment.md`](../../skill/domains/crypto/create-payment.md)
- [`../../skill/domains/crypto/query-order.md`](../../skill/domains/crypto/query-order.md)
- [`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
- [`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)

## Current published endpoint family

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
