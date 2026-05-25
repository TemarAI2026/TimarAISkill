# 公共参考

## 用途

本页是第一阶段所有 MCP 能力共享事实的公共入口。需要先确认公共运行时契约时，先看这里，再进入具体能力页。

## 公共运行时事实

当前每个接入方都应先确认这些共享事实：

- 必填请求头是 `X-Api-Key`、`X-Api-Timestamp`、`X-Api-RequestId`、`X-Api-Sign`
- 有请求体时，签名载荷是 `timestamp + requestId + rawBody`
- 无请求体时，签名载荷是 `timestamp + requestId`
- 签名公式是 `Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`
- 每次请求都应使用新的 `X-Api-RequestId`

## 公共响应事实

当前所有公开响应都使用同一个顶层包装：

- `code`
- `msg`
- `data`

当前成功判断规则：

- `code == "0"` 表示成功
- 任何其他 `code` 都表示失败，或至少需要进一步处理

建议始终保存或记录：

- `X-Api-RequestId`
- 返回的 `code`
- 返回的 `msg`
- 当涉及资金流或签名排查时，保留原始请求和响应

## 公共错误事实

遇到问题时，建议优先检查这些当前公开错误场景：

- `400`：参数无效、缺少请求头、时间戳格式不对或时间戳过期
- `401`：签名不匹配
- `500`：服务端失败
- `40009`：`merchantOrderId` 重复

已知当前公开报错文案之一：

- `Timestamp expired.`

## 能力参考入口

做当前端点和数据契约核对时，请回到这些精确 reference：

- headers：[`../../skill/references/shared/headers.md`](../../skill/references/shared/headers.md)
- 签名示例：[`../../skill/references/shared/signature-examples.md`](../../skill/references/shared/signature-examples.md)
- 错误码：[`../../skill/references/shared/error-codes.md`](../../skill/references/shared/error-codes.md)
- 端点：[`../../skill/references/crypto/endpoints.md`](../../skill/references/crypto/endpoints.md)
- 请求与响应模型：[`../../skill/references/crypto/request-response-models.md`](../../skill/references/crypto/request-response-models.md)
- 状态：[`../../skill/references/crypto/statuses.md`](../../skill/references/crypto/statuses.md)
- 集成检查清单：[`../../skill/references/crypto/integration-checklist.md`](../../skill/references/crypto/integration-checklist.md)

## 如何使用本页

推荐使用顺序：

1. 先在本页确认公共请求头和签名规则
2. 再在本页确认公共响应和错误语义
3. 再打开目标 MCP 能力页
4. 最后回到上面链接的精确 reference，完成实现前和上线前核对

## 常见错误

- 把这个摘要页当成生成最终 API 客户端代码的唯一事实来源
- 忘记 payment query 的状态当前是规范化字符串，而 payout query 当前仍是原始 `int`
- 只处理业务 payload 字段，忽略 `code` 和 `msg` 这类顶层包装字段
- 只保留商户侧订单号，不保留 `X-Api-RequestId` 这类追踪信息
