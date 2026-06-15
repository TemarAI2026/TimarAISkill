# 余额能力

## 适用范围

第一阶段 MCP 的 balance capability 当前覆盖已发布的数币账户余额查询。

## 当前接口族

- `GET /api/v2/digital/balances`

## 调用前需要准备什么

调用这一能力前，应先具备以下前提：

- 运行环境、`apiKey`、`secretKey` 与签名逻辑已经配置好
- 调用方已经明确本次查余额的用途，例如后台展示、代付前置校验或对账
- 下游代码能够区分“账户视图数据”和“订单状态数据”

## 响应概念

当前公开余额响应返回一个或多个余额项，核心字段包括：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

`availableBalance` 表示还能用于新操作的余额，`lockedBalance` 表示已经被预留或冻结的余额，`totalBalance` 是两者之和。

## 使用说明

- 应按 `currency` 维度消费余额
- 只允许使用可用余额的场景，不要误用 `totalBalance`
- 不要把一次余额读取当成代付并发控制的唯一依据
- 如果余额会影响资金决策，建议保留足够的快照或日志，便于排查和对账

## 常见错误

- 把 `totalBalance` 当成全部可用余额
- 忽略 `lockedBalance`
- 用查余额替代业务侧的资金预留逻辑
- 把余额数据和订单查询结果混在同一套处理逻辑里

## 精确参考

- [`../../skill/domains/crypto/query-balance.md`](../../skill/domains/crypto/query-balance.md)
- [`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
- [`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)
- [`../../skill/references/crypto/integration-checklist.md`](../../skill/references/crypto/integration-checklist.md)
