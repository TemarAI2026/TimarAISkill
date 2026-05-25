# Domain Map

## Current document layers

公开集成文档按少量层次组织，方便读者从入口逐步走到实现细节：

- Hub 文档，例如 [Integration Router](./integration-router.md)，用于帮助选择阅读路径。
- Shared 共享文档，例如 [Auth Signing](../domains/shared/auth-signing.md)、[Error Handling](../domains/shared/error-handling.md) 与 [Response Conventions](../domains/shared/response-conventions.md)，说明跨能力复用的规则。
- Domain 业务域文档，例如 [Crypto Overview](../domains/crypto/overview.md)，说明特定业务域下当前已发布的运行时能力流程。
- Reference 参考文档，例如 [crypto endpoints](../references/crypto/endpoints.md)、[request and response models](../references/crypto/request-response-models.md) 与 [shared headers](../references/shared/headers.md)，提供实现细节。
- Examples 示例文档，例如 [Node.js crypto example](../examples/crypto/nodejs/README.md)、[C# crypto example](../examples/crypto/csharp/README.md) 与 [cURL crypto example](../examples/crypto/curl/README.md)，展示端到端使用方式。

## MCP-first interpretation

- MCP 是当前公开文档背后的能力底座。
- `skill`、`x402` 与未来其他协议面应路由到 MCP，而不是各自维护独立业务核心。
- 当前 `docs/skill` 树应理解为“如何使用基于 MCP 的能力”的公开引导层。

## Available domains

- Shared：在任何业务流程之前或过程中都可能用到的跨能力规则，包括签名、响应约定与错误处理。
- Crypto：当前已发布业务域，涵盖已公开数币端点的支付、代付、订单查询、Webhook 与余额能力说明。
- Fiat：保留给未来的业务域。入口文件已存在于 [Fiat README](../domains/fiat/README.md)，但当前尚未发布法币流程、字段集合或状态映射。

## Expansion note

未来新增业务域时，Shared 层仍作为共同基础，而各业务域再补上自己的指南、参考文档与示例。外部接入方与 AI 编程助手应先从 hub 开始，确认能力路径后，再依次阅读对应的 shared 与 domain 文档，然后再生成代码。
