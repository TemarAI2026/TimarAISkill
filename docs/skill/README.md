# AI Integration Docs

## Positioning

本目录是面向 AI 编程助手与外部接入方的公开入口，用于帮助读者理解以 MCP 为底座的开放能力模型、当前已发布业务域，以及实现前的推荐阅读顺序。

## MCP-first model

- `MCP` 是运行时开放能力的主要底座。
- `skill` 是面向 AI 的引导与触发层。
- `x402` 与后续其他支付协议应作为 MCP 之上的适配层，而不是各自维护独立业务核心。

## Who this is for

- 使用 Claude Code、Cursor 或 Codex 协助接入支付能力的开发者
- 需要快速定位能力文档、参考资料与实现约束的外部接入方
- 希望在生成代码前先建立正确上下文的工程团队

## Supported AI coding assistants

- Claude Code
- Cursor
- Codex

## Quick start

建议在生成或修改集成代码前，按以下顺序阅读：

1. 从 [`hub/integration-router.md`](./hub/integration-router.md) 开始，确认对应的 MCP 能力路径与推荐阅读顺序。
2. 继续阅读 [`hub/domain-map.md`](./hub/domain-map.md)，了解当前已发布业务域与文档布局。
3. 进入当前业务域文档。当前已发布运行时业务域为 [`domains/crypto/overview.md`](./domains/crypto/overview.md)。
4. 再查阅共享规则文档，例如 [`domains/shared/auth-signing.md`](./domains/shared/auth-signing.md)、[`domains/shared/error-handling.md`](./domains/shared/error-handling.md) 与 [`domains/shared/response-conventions.md`](./domains/shared/response-conventions.md)。
5. 最后读取参考资料，再开始生成代码，例如 [`references/crypto/integration-checklist.md`](./references/crypto/integration-checklist.md)、[`references/crypto/endpoints.md`](./references/crypto/endpoints.md) 与 [`references/shared/headers.md`](./references/shared/headers.md)。

## Current domain availability

- `Crypto`：当前已发布，可用于基于 MCP 的集成规划与代码生成。
- `Fiat`：已预留未来业务域入口，当前不应视为已实现。

## Directory guide

- [`hub/`](./hub/)：总览入口、路由说明与 MCP 能力导航。
- [`domains/`](./domains/)：按当前已发布业务域组织的实现说明。
- [`domains/crypto/`](./domains/crypto/)：当前可用的数币能力文档。
- [`domains/fiat/`](./domains/fiat/)：未来法币文档的预留位置。
- [`domains/shared/`](./domains/shared/)：认证、错误处理、响应约定等跨能力共享规则。
- [`references/`](./references/)：端点、模型、状态、请求头与检查清单等参考资料。
- [`examples/`](./examples/)：用于辅助实现理解的示例资产。

## Important usage note

这些文档的目标是在代码生成前建立正确上下文。请先阅读 router 与当前已发布业务域说明，再结合共享规则和参考资料实现代码；不要只根据单一接口页面直接生成集成逻辑。
