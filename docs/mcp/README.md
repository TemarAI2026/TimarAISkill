# MCP Public Docs

## Positioning

本目录是 Timar 平台面向外部接入方与 AI 编程助手的 MCP 公共文档入口。

在当前架构里：

- `MCP` 是开放能力底座
- `skill` 是 AI 引导与触发层
- `x402` 与未来协议面是 MCP 上层适配层

## Who this is for

- 需要对接 Timar 开放能力的外部商户或技术团队
- 使用 Claude Code、Cursor、Codex 等 AI 编程助手辅助接入的开发者
- 需要快速理解 MCP 能力边界与调用路径的解决方案工程师

## Runtime prerequisites

本目录默认以下前置条件已经完成：

- 商户注册与权限开通
- 必要的业务准入与内部审核
- API 凭证已分配

运行时接入重点不在开户流程，而在：

- 选择环境
- 配置 `apiKey` 与 `secretKey`
- 生成签名相关参数
- 调用 MCP 能力

## Reading order

1. 先读 [`overview/architecture.md`](./overview/architecture.md)
2. 再读 [`overview/environments.md`](./overview/environments.md)
3. 再读 [`overview/auth-signing.md`](./overview/auth-signing.md)
4. 然后按能力进入：
   - [`capabilities/payment.md`](./capabilities/payment.md)
   - [`capabilities/payout.md`](./capabilities/payout.md)
   - [`capabilities/balance.md`](./capabilities/balance.md)
   - [`capabilities/notifications.md`](./capabilities/notifications.md)
5. 最后用 [`references/common.md`](./references/common.md) 与现有 `docs/skill/references/` 做精确核对

## Relationship to `docs/skill`

`docs/mcp/` 是 MCP-first 的公共入口层。  
`docs/skill/` 继续保留，作为 AI-facing 的能力引导层与当前已发布数币能力说明层。

第一阶段中：

- `docs/mcp/` 负责总体说明与能力路由
- `docs/skill/references/` 继续承载当前已发布能力的精确事实层
