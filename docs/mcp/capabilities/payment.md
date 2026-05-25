# 收单能力

## 适用范围

第一阶段 MCP 的 payment capability 当前覆盖的是数币收单订单相关操作。当前已发布的操作集合包括：

- 创建收单订单
- 查询收单订单
- 取消收单订单

## 当前接口族

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`

## 调用前需要准备什么

调用这一能力前，应先具备以下前提：

- 商户入驻和账户配置已经完成
- 运行环境、`apiKey`、`secretKey` 与签名逻辑已经配置好
- 业务系统能够生成唯一的 `merchantOrderId`
- 业务系统已经明确 `merchantUserId`、`amount`、`currency` 和 `network`

这一层 MCP 默认商户身份和上游业务上下文都已经准备完成。

## 创建订单

使用 `POST /api/v2/digital/payments` 创建新的收单订单。

当前请求概念包括：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 可选：`returnUrl`
- 可选：`cancelUrl`

当前创建响应里，调用方通常需要持久化或消费这些字段：

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## 查询与取消

使用 `GET /api/v2/digital/payments/{orderId}` 获取订单当前状态。

当前查询响应会暴露这类关键信息：

- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

使用 `POST /api/v2/digital/payments/{orderId}/cancel` 时，只应传入你真正要取消的平台 `orderId`。不要假设 `merchantOrderId` 可以直接替代 `orderId`。

## 状态消费说明

当前 payment query 的状态在公开响应里以规范化字符串返回，典型值包括：

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

建议持久化原始响应，并为未来新增状态值保留安全兜底逻辑。

## 常见错误

- 在当前创建收单请求里臆造 `callbackUrl`
- 把 `merchantOrderId` 和平台 `orderId` 当成同一个标识
- 把 `currency` 与 `network` 混成一个字段
- 把 `returnUrl` 或 `cancelUrl` 当成服务端异步通知地址
- 忽略 `expiresInSeconds`，仍然对外展示已过期的支付订单

## 精确参考

- [`../../skill/domains/crypto/overview.md`](../../skill/domains/crypto/overview.md)
- [`../../skill/domains/crypto/create-payment.md`](../../skill/domains/crypto/create-payment.md)
- [`../../skill/domains/crypto/query-order.md`](../../skill/domains/crypto/query-order.md)
- [`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
- [`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)
- [`../../skill/references/crypto/statuses.md`](../../skill/references/crypto/statuses.md)
