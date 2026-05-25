# MCP Architecture

## MCP role

MCP 是 Timar 平台开放能力的统一底座。

它的职责是：

- 提供一致的开放能力入口
- 统一运行时调用模型
- 统一签名、请求标识、错误语义与能力路由

## Relationship to upper layers

- `skill`：AI-facing 的引导与触发层
- `x402`：协议适配层
- 未来其他协议：同样应作为 MCP 上层适配层

这些上层入口不应各自维护独立业务核心，而应路由到 MCP。

## What MCP owns in phase one

- payment capability path
- payout capability path
- balance capability path
- notification capability path
- common runtime calling conventions

## What MCP does not own in phase one

- 商户注册流程
- KYB/KYC 过程本身
- 内部钱包实现细节
- settlement / ledger 内部逻辑
- 内部对账和运营流程

这些要么属于前置条件，要么属于下层平台实现。
