# AI Integration Docs

## Positioning

本目录是面向 AI 编码助手与外部集成方的公开入口，用于快速理解 VGPAY 的集成文档结构、可用业务域与推荐阅读顺序。

## Who this is for

- 使用 Claude Code、Cursor 或 Codex 协助接入支付能力的开发者
- 需要快速定位业务域文档、参考资料与实现约束的外部集成方
- 希望在生成代码前先建立正确上下文的工程团队

## Supported AI coding assistants

- Claude Code
- Cursor
- Codex

## Quick start

建议在生成或修改集成代码前，按以下顺序阅读：

1. 从 [`hub/integration-router.md`](./hub/integration-router.md) 开始，确认任务入口与推荐阅读路径。
2. 继续阅读 [`hub/domain-map.md`](./hub/domain-map.md)，了解当前业务域与文档分布。
3. 进入当前业务域文档。当前已实现业务域为 [`domains/crypto/overview.md`](./domains/crypto/overview.md)。
4. 再查阅共享规则文档，例如 [`domains/shared/auth-signing.md`](./domains/shared/auth-signing.md)、[`domains/shared/error-handling.md`](./domains/shared/error-handling.md) 与 [`domains/shared/response-conventions.md`](./domains/shared/response-conventions.md)。
5. 最后读取参考资料，再开始生成代码，例如 [`references/crypto/integration-checklist.md`](./references/crypto/integration-checklist.md)、[`references/crypto/endpoints.md`](./references/crypto/endpoints.md) 与 [`references/shared/headers.md`](./references/shared/headers.md)。

## Current domain availability

- `Crypto`：当前已实现并可用于接入规划与代码生成。
- `Fiat`：已预留目录与入口，后续扩展时使用，当前不应视为已实现。

## Directory guide

- [`hub/`](./hub/)：总览入口、路由说明与业务域映射。
- [`domains/`](./domains/)：按业务域组织的实现说明。
- [`domains/crypto/`](./domains/crypto/)：当前可用的加密货币集成文档。
- [`domains/fiat/`](./domains/fiat/)：法币业务域预留位置。
- [`domains/shared/`](./domains/shared/)：跨业务域共用的认证、错误处理与响应约定。
- [`references/`](./references/)：接口端点、字段模型、状态、请求头与校验清单等参考资料。
- [`examples/`](./examples/)：示例资产目录，可用于补充实现理解。

## Important usage note

这些文档的设计目标是帮助你先建立正确上下文，再进行代码生成。请先阅读路由与业务域说明，再结合共享规则和参考资料实现代码；不要只根据单一接口页面直接产出集成逻辑。
