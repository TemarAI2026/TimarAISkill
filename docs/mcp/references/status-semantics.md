# 状态语义

## 用途

本页说明外部接入方和 AI 编程助手应如何解释当前公开的 payment 与 payout 状态语义。

## Payment 状态语义

当前 payment query 在公开响应里返回的是规范化字符串状态，典型包括：

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

下游系统应以这些公开返回值作为消费对象。

## Payout 状态语义

当前 payout query 在公开响应里返回的是原始 `int` 状态值。

当前映射示例包括：

- `0`：`PENDING_BUSINESS_APPROVAL`
- `1`、`2`、`3`、`6`、`7`、`8`、`9`：pending 家族状态
- `4`：`CANCEL`
- `5`：`SUCCESS`

## 消费规则

- 不要假设 payment 和 payout 的状态在线路格式上完全一致
- 应持久化平台返回的原始状态值或原始状态字符串
- payout 的 `int` 值应通过当前公开状态表做映射
- 对未来新增未知状态，应保留日志和安全处理逻辑

## 常见错误

- 把 payout 的 `status` 当成已经规范化的字符串
- 把所有非成功状态都压扁成同一个失败桶
- 本地归一化后，丢掉平台原始状态值
- 假设 payment 和 payout 状态语义完全一样

## 精确来源参考

- [`../../skill/references/crypto/statuses.md`](../../skill/references/crypto/statuses.md)
