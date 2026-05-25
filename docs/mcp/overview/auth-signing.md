# 鉴权与签名

## 适用范围

本页定义第一阶段 MCP 运行时请求鉴权的最小公开契约。无论是人工接入方还是 AI 编程助手，在调用任何已发布 API 能力前，都应先满足这里的规则。

## 每次请求都要准备的运行时输入

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

不要复用旧的 `requestId`、时间戳或签名。

## 必填请求头

当前公开调用要求完整携带以下四个请求头：

- `X-Api-Key`
- `X-Api-Timestamp`
- `X-Api-RequestId`
- `X-Api-Sign`

`X-Api-Timestamp` 当前接受 Unix 毫秒时间戳或 ISO8601。已知公开报错之一是 `Timestamp expired.`，因此客户端时间需要和服务端保持对齐，且不要拿过期请求重放。

## 签名载荷规则

- 有请求体时：`timestamp + requestId + rawBody`
- 无请求体时：`timestamp + requestId`

这里的 `rawBody` 指真正发到线上线路中的原始字符串。应该对最终序列化结果签名，而不是对对象重建后的内容签名。

## 签名公式

```text
Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))
```

## 运行时检查清单

发送请求前建议按这个顺序检查：

1. 生成新的 `requestId`
2. 先得到最终要发送的请求体原始字符串，如果该请求有 body
3. 按上面的规则拼出签名载荷
4. 生成 `X-Api-Sign`
5. 发送请求后不要再改写 body

## 不要猜测

- 不要擅自把 HTTP method、path、query string、分隔符或换行拼进签名，除非公开契约明确要求
- 不要把空 body 替换成 `{}` 或其他占位值
- 不要签一个 JSON，再发送另一个 JSON
- 不要记录 `secretKey`
- 不要自行假设比当前文档更宽松的时间窗口

## 精确参考

实际实现或代码审查时，请回到以下页面核对：

- [`../../skill/references/shared/headers.md`](../../skill/references/shared/headers.md)
- [`../../skill/references/shared/signature-examples.md`](../../skill/references/shared/signature-examples.md)
- [`../../skill/domains/shared/auth-signing.md`](../../skill/domains/shared/auth-signing.md)
