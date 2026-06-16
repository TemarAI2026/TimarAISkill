# MCP 公开文档

## 定位

这个目录是 Temar 平台面向外部接入方与 AI 编程助手的 MCP 公开文档入口。

在当前架构中：

- `MCP` 是开放能力底座
- `skill` 是面向 AI 的引导与触发层
- `x402` 以及未来其他协议面，都是建立在 MCP 之上的上层适配层

如果未来有新的接入面需要复用收单、代付或公共签名逻辑，应优先路由进 MCP，而不是再维护一套独立业务内核。

## 面向对象

- 需要对接 Temar 开放能力的外部商户或工程团队
- 使用 Codex、Claude Code、Cursor 等 AI 编程助手做接入的开发者
- 需要快速理解 MCP 能力边界和调用路径的解决方案工程师

## 这些文档默认你已经具备的前提

这些文档默认在开始运行时接入前，以下工作已经完成：

- 商户注册和接入开通
- 必要的业务准入与内部审核
- API 凭证已经签发

运行时阶段真正关注的是：

- 选择目标环境
- 配置 `apiKey` 和 `secretKey`
- 生成签名输入
- 调用 MCP 能力
- 正确保存返回的标识和状态

## 快速接入路径

如果你是从 0 开始接入，建议按这个顺序阅读：

1. 先读 [`overview/architecture.md`](./overview/architecture.md)，理解 MCP 管什么、不管什么
2. 再读 [`overview/environments.md`](./overview/environments.md)，理解环境隔离和运行时假设
3. 在生成任何客户端代码前，先读 [`overview/auth-signing.md`](./overview/auth-signing.md)
4. 再按需要进入能力页：
   - [`capabilities/payment.md`](./capabilities/payment.md)
   - [`capabilities/payout.md`](./capabilities/payout.md)
   - [`capabilities/balance.md`](./capabilities/balance.md)
   - [`capabilities/notifications.md`](./capabilities/notifications.md)
5. 最后结合 [`references/common.md`](./references/common.md) 和当前精确 reference 页面做契约核对

## 第一阶段能力范围

当前公开 MCP 能力范围包括：

- 收单
- 代付
- 余额
- 通知
- 公共运行时调用约定

当前第一阶段不纳入公开 MCP 主范围的内容包括：

- 商户注册流程
- KYB / KYC 处理流程
- settlement / ledger 内部实现
- wallet 内部实现细节
- 内部对账和运营处置流程
- 已发布响应契约之外的链上执行细节

## 运行时五步检查

无论是人工接入还是让 AI 编程助手生成代码，建议都按这个顺序推进：

1. 先确认目标环境和凭证
2. 再确认请求头和签名输入
3. 再确认具体能力的请求字段和端点路径
4. 正确保存平台返回的标识、状态值和追踪信息
5. 为查单、重试、取消和异步通知准备安全的后续处理逻辑

## 常见错误

- 把 `docs/mcp/` 当成商户开户文档，而不是运行时接入文档
- 自行臆造当前公开模型中不存在的请求字段
- 把收单和代付混成一套泛化流程
- 忽略 create 接口返回的平台 `orderId`
- 只看摘要说明，不回到精确 reference 做最终核对

## 与 `docs/skill` 的关系

`docs/mcp/` 是 MCP-first 的公开入口层。
`docs/skill/` 继续保留，作为面向 AI 的引导层，以及当前已发布数币能力的精确 reference 层。

在当前阶段：

- `docs/mcp/` 负责总体说明、接入路径和能力导航
- `docs/skill/references/` 继续承载当前已发布能力的精确事实层

推荐的用法是：先从 `docs/mcp/` 入门，再回到精确 reference 完成实现和上线前核对。
