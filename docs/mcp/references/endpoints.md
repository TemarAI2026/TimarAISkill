# 端点参考

## 用途

本页列出第一阶段数币能力集中当前已发布的 MCP-facing 端点族。

## 当前已发布端点

| Method | Path | 能力 | 用途 |
| --- | --- | --- | --- |
| `POST` | `/api/v2/digital/payments` | `payment` | 创建收单订单。 |
| `GET` | `/api/v2/digital/payments/{orderId}` | `payment` | 查询收单订单。 |
| `POST` | `/api/v2/digital/payments/{orderId}/cancel` | `payment` | 取消收单订单。 |
| `POST` | `/api/v2/digital/payouts` | `payout` | 创建代付订单。 |
| `GET` | `/api/v2/digital/payouts/{orderId}` | `payout` | 查询代付订单。 |
| `GET` | `/api/v2/digital/balances` | `balance` | 查询账户余额。 |

## 使用说明

- payment 和 payout 不是共用一个 create 端点
- 当前查单路径使用的是平台 `orderId` 路径参数
- 当前公开 cancel 只定义在 payment 订单上
- 余额查询是账户视图端点，不是订单端点

## 常见错误

- 用 payout 的请求字段去调用 payment 路径
- 明明公开路径要求平台 `orderId`，却用 `merchantOrderId` 去查
- 因为 payment 有 cancel，就默认 payout 也有公开 cancel
- 把余额查询当成订单状态查询使用

## 精确来源参考

- [`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
