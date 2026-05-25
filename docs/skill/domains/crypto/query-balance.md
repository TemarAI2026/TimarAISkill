# 查询余额

## 这个接口返回什么

使用 `GET /api/v2/digital/balances` 查询当前账户的币种余额视图。当前公开响应概念包括：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

其中，`availableBalance` 表示可用于新操作的余额，`lockedBalance` 表示已被占用或冻结的余额，`totalBalance` 是两者合计。

## 如何安全使用

把余额查询当作账户视图，而不是单笔订单结果。它适合用于：

- 提交代付前做余额预检查
- 后台展示每个币种的可用余额和冻结余额
- 对账或监控余额变化趋势

不要仅依赖一次余额读取来决定高并发出款结果。你的业务层仍应处理并发提交、额度预留和失败回滚等问题。

## 实现前先读

实现前建议先确认：

- 你的代码按 `currency` 维度处理余额记录
- 你的界面或服务清楚区分 `availableBalance` 和 `totalBalance`
- 你已经阅读 [加密货币端点参考](../../references/crypto/endpoints.md)
- 你已经阅读 [请求与响应模型](../../references/crypto/request-response-models.md)
