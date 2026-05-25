# Authentication and Signing

## Runtime credentials

MCP 第一阶段运行时接入，重点围绕以下输入：

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

## What to read next

本页只负责 MCP 视角下的总说明。  
精确契约事实仍应回到当前已发布参考资料核对：

- [`../../skill/references/shared/headers.md`](../../skill/references/shared/headers.md)
- [`../../skill/references/shared/signature-examples.md`](../../skill/references/shared/signature-examples.md)
- [`../../skill/domains/shared/auth-signing.md`](../../skill/domains/shared/auth-signing.md)

## Runtime rule

在当前公开能力集中：

- 每次请求都应生成新的 `requestId`
- 签名应基于当前请求真实发送的原始内容
- 不应在签名后再重组或重格式化请求体
