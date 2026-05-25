# 创建支付

## 何时使用这个接口

当你需要为用户创建一笔新的加密货币收款订单时，使用 `POST /api/v2/digital/payments`。这个接口适合需要生成收款链接、收款地址和订单有效期信息的场景。

当前创建成功后，响应通常会包含 `orderId`、`merchantOrderId`、`paymentUrl`、`amount`、`receiveAddress`、`currency`、`network`、`expiresInSeconds`。

## 需要确认的请求字段

当前支付创建模型字段如下：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 选填：`returnUrl`
- 选填：`cancelUrl`

在生成代码前，先确认：

- `merchantOrderId` 在你的商户侧是否唯一
- `merchantUserId` 是否对应你自己的用户标识
- `amount`、`currency`、`network` 是否来自当前允许的业务配置
- `returnUrl` 和 `cancelUrl` 是否只是前端跳转地址，而不是服务端异步通知地址

## 生成代码前

先把这些上下文放进实现说明或提示词里：

- 目标端点是 `POST /api/v2/digital/payments`
- 当前请求模型不包含 `callbackUrl`
- 代码需要消费创建响应里的 `orderId`、`paymentUrl`、`receiveAddress`、`expiresInSeconds`
- 后续查询使用 [查询订单](./query-order.md) 和 [状态参考](../../references/crypto/statuses.md)
- 签名、错误处理、响应结构要结合共享文档一起实现

建议同时阅读：

- [加密货币端点参考](../../references/crypto/endpoints.md)
- [请求与响应模型](../../references/crypto/request-response-models.md)
- [鉴权与签名](../shared/auth-signing.md)
- [响应约定](../shared/response-conventions.md)

## 常见错误

- 把 `callbackUrl` 当作当前支付创建字段使用
- 把 `merchantOrderId` 当作平台返回的 `orderId` 使用
- 漏传 `network`，或者把链名和币种混为一个字段
- 把 `returnUrl`、`cancelUrl` 当作 webhook 地址
- 创建后不处理 `expiresInSeconds`，导致过期订单仍被继续展示给用户
