# 请求与响应模型

## 用途

本页汇总当前 MCP-facing 的收单、代付、余额客户端实现时最需要关注的公开请求与响应模型形态。

## 公共顶层响应包装

当前所有公开响应都使用同一个顶层包装：

| 字段 | 含义 |
| --- | --- |
| `code` | 业务结果码。成功固定为 `"0"`。 |
| `msg` | 可读结果或错误消息。 |
| `data` | 具体业务负载，形态取决于端点。 |

## Payment create

当前请求字段：

- 必填：`merchantOrderId`、`merchantUserId`、`amount`、`currency`、`network`
- 可选：`returnUrl`、`cancelUrl`

当前建议保留的响应字段：

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## Payment query

当前常用响应字段：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

## Payout create

当前请求字段：

- 必填：`merchantOrderId`、`merchantUserId`、`amount`、`currency`、`network`、`withdrawAddress`

当前建议保留的响应字段：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## Payout query

当前常用响应字段：

- `orderId`
- `merchantOrderId`
- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

## Balance query

当前响应项字段：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

## 常见错误

- 只处理 `data`，忽略 `code` 和 `msg`
- 在当前未发布的 create 请求里臆造 `callbackUrl`
- 只保留 `merchantOrderId`，丢掉平台 `orderId`
- 把余额字段当成订单字段来处理

## 精确来源参考

- [`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)
