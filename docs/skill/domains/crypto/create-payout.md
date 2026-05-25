# 创建代付

## 何时使用这个接口

当你需要从平台向外部钱包地址发起一笔新的加密货币出款时，使用 `POST /api/v2/digital/payouts`。这不是收款流程的镜像版本；它是独立的出款流程，重点在出款地址、费用和后续状态追踪。

当前创建成功后，响应通常会包含 `orderId`、`merchantOrderId`、`merchantUserId`、`amount`、`fee`、`currency`、`network`、`withdrawAddress`。

## 需要确认的请求字段

当前代付创建模型字段如下：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 必填：`withdrawAddress`

在生成代码前，先确认：

- `withdrawAddress` 是否来自正确链路，并与 `network` 匹配
- `merchantOrderId` 在你的商户侧是否唯一
- `merchantUserId` 是否满足你的审计或用户归属需求
- `amount` 和余额策略是否已经在你的业务层做过前置校验

## 生成代码前

先把这些上下文放进实现说明或提示词里：

- 目标端点是 `POST /api/v2/digital/payouts`
- 当前请求模型不包含 `callbackUrl`
- 代码需要消费创建响应里的 `orderId`、`fee`、`withdrawAddress`
- 后续查询使用 [查询订单](./query-order.md) 和 [状态参考](../../references/crypto/statuses.md)
- 代付和支付是不同流程，不应复用支付专用字段如 `returnUrl`、`cancelUrl`

建议同时阅读：

- [加密货币端点参考](../../references/crypto/endpoints.md)
- [请求与响应模型](../../references/crypto/request-response-models.md)
- [查询余额](./query-balance.md)
- [错误处理](../shared/error-handling.md)

## 常见错误

- 漏传 `withdrawAddress`，或地址与 `network` 不匹配
- 把支付接口和代付接口当成同一个流程的两个状态
- 在当前代付创建请求里添加 `callbackUrl`
- 复用支付场景里的 `returnUrl`、`cancelUrl`
- 只记录 `merchantOrderId`，却不保存平台返回的 `orderId` 用于后续查询
