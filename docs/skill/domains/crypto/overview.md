# 加密货币总览

## 本领域涵盖什么

Crypto 领域面向公开的数字资产集成能力，帮助外部集成方和 AI 编码助手理解当前可用的 V2 接口、字段约束，以及支付和代付的基本流程差异。

当前公开接口包括：

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`

## 支持的任务

- 创建加密货币支付订单，并获取 `paymentUrl`、`receiveAddress`、`expiresInSeconds`
- 查询支付订单状态和支付明细
- 取消尚未完成的支付订单
- 创建加密货币代付订单，并跟踪提币结果
- 查询账户余额，包括 `availableBalance`、`lockedBalance`、`totalBalance`
- 规划 webhook 接收端，但在实现前先核对当前公开文档和你的集成约定

## 支付与代付的区别

支付是收款流程，面向用户向平台地址付款，入口是 `POST /api/v2/digital/payments`。创建后通常会使用 `paymentUrl` 或 `receiveAddress` 引导付款，并关注订单是否在有效期内完成。

代付是出款流程，面向平台向外部地址打款，入口是 `POST /api/v2/digital/payouts`。代付请求必须明确 `withdrawAddress`，并且更关注地址正确性、余额是否足够、以及链上出款后的状态追踪。

不要混用这两个流程的字段或心智模型。例如，`returnUrl` 和 `cancelUrl` 只属于当前支付创建模型，`withdrawAddress` 只属于当前代付创建模型。

## 下一步阅读

- [创建支付](./create-payment.md)
- [创建代付](./create-payout.md)
- [查询订单](./query-order.md)
- [查询余额](./query-balance.md)
- [处理 webhook](./handle-webhook.md)
- [加密货币端点参考](../../references/crypto/endpoints.md)
- [请求与响应模型](../../references/crypto/request-response-models.md)
