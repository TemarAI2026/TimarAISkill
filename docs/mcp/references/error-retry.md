# 错误与重试说明

## 用途

本页汇总当前 MCP-facing 接入中的公共错误处理与重试边界。

## 建议记录什么

应始终保留：

- `X-Api-RequestId`
- 请求路径和方法
- `X-Api-Timestamp`
- 响应 `code`
- 响应 `msg`
- 原始请求体的安全副本或 body hash

不要记录 `secretKey`。

## 优先排查顺序

出错后，建议至少先检查：

1. 四个必填请求头是否齐全
2. 时间戳格式和时效性
3. 用于签名的原始 body 是否与实际发送一致
4. API key 与 secret key 是否配对正确
5. 请求是否可能已经被平台接受

## 当前常见公开错误场景

- `400`：参数无效、缺请求头、时间戳无效或已过期
- `401`：签名不匹配
- `500`：服务端失败
- `40007`：收单订单不存在
- `40008`：代付订单不存在
- `40009`：`merchantOrderId` 重复

已知当前公开错误文案之一：

- `Timestamp expired.`

## 重试规则

- 不要盲目重试 create-payment 或 create-payout
- 先用自己的订单号和已保存的 `X-Api-RequestId` 做调查
- 对重复提交使用短期去重
- 只有在确认问题不是由签名、请求头或重复业务标识导致时，才进入下一步重试判断

## 常见错误

- 把 create 请求当成天然安全且天然幂等，直接重试
- 排查时不保留 `X-Api-RequestId`
- 记录了敏感密钥，却没记录追踪信息
- 看到 `500` 就直接认定一定没有产生下游副作用

## 精确来源参考

- [`../../skill/domains/shared/error-handling.md`](../../skill/domains/shared/error-handling.md)
- [`../../skill/references/shared/error-codes.md`](../../skill/references/shared/error-codes.md)
