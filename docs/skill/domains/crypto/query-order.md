# 查询订单

## 可以查询什么

这个指南同时适用于两类查询：

- 支付订单：`GET /api/v2/digital/payments/{orderId}`
- 代付订单：`GET /api/v2/digital/payouts/{orderId}`

支付查询通常用于确认订单状态、已支付金额、收款地址、支付链接、有效期和入账明细。代付查询通常用于确认订单状态、手续费、链上交易号、出款地址和来源地址。

## 实现前先读

在代码里把支付查询和代付查询当作两个独立接口处理，即使它们都使用 `orderId` 路径参数。当前公开模型里，支付查询和代付查询返回的状态类型和扩展字段并不相同。

实现前建议先确认：

- 你保存了创建响应返回的 `orderId`，而不只保存 `merchantOrderId`
- 你已经阅读对应的 [状态参考](../../references/crypto/statuses.md)
- 你已经阅读 [请求与响应模型](../../references/crypto/request-response-models.md)
- 取消支付时，先结合支付查询确认当前订单状态，再调用 `POST /api/v2/digital/payments/{orderId}/cancel`

## 常见错误

- 用支付查询逻辑直接解析代付返回，或反过来
- 只按 `merchantOrderId` 建表，却没有保存平台 `orderId`
- 假设支付和代付使用完全相同的状态值或状态类型
- 未先读取状态文档，就把某个状态直接当成终态或成功态
