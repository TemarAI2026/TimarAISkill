# 请求头参考

## 请求头一览

| Header | 必填 | 说明 |
| --- | --- | --- |
| `X-Api-Key` | 是 | 商户 API key。 |
| `X-Api-Timestamp` | 是 | 请求时间，支持 Unix 毫秒时间戳或 ISO8601。 |
| `X-Api-RequestId` | 是 | 请求唯一标识，用于追踪、排障和短期去重。 |
| `X-Api-Sign` | 是 | 按当前签名规则计算出的 Base64 签名值。 |

## 说明

- 每次请求都应生成新的 `X-Api-RequestId`。
- 有请求体时，签名原文是 `timestamp + requestId + rawBody`。
- 无请求体时，签名原文是 `timestamp + requestId`。
- 当前公开错误信息已知包含 `Timestamp expired.`，通常表示时间戳格式或时效窗口需要先检查。
- 公开文档建议对 `X-Api-RequestId` 做短期去重，以降低重放风险。
