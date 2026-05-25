# 代付能力

## 适用范围

第一阶段 MCP 的 payout capability 当前覆盖的是数币代付订单相关操作。当前已发布的操作集合包括：

- 创建代付订单
- 查询代付订单

## 当前接口族

- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`

## 调用前需要准备什么

调用这一能力前，应先具备以下前提：

- 商户入驻和账户配置已经完成
- 运行环境、`apiKey`、`secretKey` 与签名逻辑已经配置好
- 业务系统能够生成唯一的 `merchantOrderId`
- 业务系统已经明确 `merchantUserId`、`amount`、`currency`、`network` 和 `withdrawAddress`
- 余额校验、提交权限和业务侧放行策略，应该由你自己的业务层先做决定

这一层 MCP 不能替代你自己的审批、风控或资金预留逻辑。

## 创建订单

使用 `POST /api/v2/digital/payouts` 创建新的代付订单。

当前请求概念包括：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 必填：`withdrawAddress`

当前创建响应里，调用方通常需要持久化或消费这些字段：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## 查询订单

使用 `GET /api/v2/digital/payouts/{orderId}` 获取代付当前状态。

当前查询响应会暴露这类关键信息：

- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

请同时保存平台 `orderId` 和你自己的 `merchantOrderId`，否则后续查单与对账会受影响。

## 状态跟进说明

当前 payout query 的状态在公开响应里以原始 `int` 值返回。应按照已发布的 payout 状态表做映射，并为未来未知状态保留安全处理逻辑。

不要把“创建成功返回”直接当成“代付最终完成”。业务侧通常仍需要轮询或结合异步通知继续确认。

## 常见错误

- 漏传 `withdrawAddress`，或地址与 `network` 不匹配
- 把 payout 当成 payment 的镜像流程
- 混入 `returnUrl`、`cancelUrl`、`callbackUrl` 这类 payment 才有的字段
- 只保存 `merchantOrderId`，丢掉平台 `orderId`
- 把首次创建响应直接当成最终完成状态

## 精确参考

- [`../../skill/domains/crypto/overview.md`](../../skill/domains/crypto/overview.md)
- [`../../skill/domains/crypto/create-payout.md`](../../skill/domains/crypto/create-payout.md)
- [`../../skill/domains/crypto/query-order.md`](../../skill/domains/crypto/query-order.md)
- [`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
- [`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)
- [`../../skill/references/crypto/statuses.md`](../../skill/references/crypto/statuses.md)
