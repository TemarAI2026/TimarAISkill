# Integration Router

## How to use this router

使用本页作为编写集成代码或提示 AI 编程助手前的第一站。这个 router 的目标，是帮助你先选择正确的 MCP 能力路径，再进入对应的共享规则与当前已发布业务域说明。

如果你刚接触当前公开 API 面，请先读 [Domain Map](./domain-map.md)，再回到这里。

## Capability-based routes

| Task | Read next | Why this path |
| --- | --- | --- |
| Sign a request | [Shared signing guide](../domains/shared/auth-signing.md), [Shared headers reference](../references/shared/headers.md), [Signature examples](../references/shared/signature-examples.md) | 说明 MCP 背后共享签名流程，以及调用能力时需要的请求头。 |
| Create a crypto payment | [Crypto overview](../domains/crypto/overview.md), [Create payment](../domains/crypto/create-payment.md), [Crypto endpoints](../references/crypto/endpoints.md), [Request and response models](../references/crypto/request-response-models.md) | 覆盖当前通过数币业务域文档公开的支付能力路径。 |
| Query a payment order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Crypto statuses](../references/crypto/statuses.md) | 帮助理解当前公开的支付查单能力路径与回传状态。 |
| Cancel a payment order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Error handling](../domains/shared/error-handling.md) | 指向当前公开的支付取消能力路径及其错误处理要求。 |
| Create a crypto payout | [Crypto overview](../domains/crypto/overview.md), [Create payout](../domains/crypto/create-payout.md), [Crypto endpoints](../references/crypto/endpoints.md), [Request and response models](../references/crypto/request-response-models.md) | 覆盖当前数币业务域公开的代付能力路径。 |
| Query a payout order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Crypto statuses](../references/crypto/statuses.md) | 适用于当前公开的代付查单能力路径与状态含义。 |
| Query balance | [Crypto overview](../domains/crypto/overview.md), [Query balance](../domains/crypto/query-balance.md), [Crypto endpoints](../references/crypto/endpoints.md) | 覆盖当前公开的余额能力路径及其数币账户上下文。 |
| Understand callback and webhook handling | [Handle webhook](../domains/crypto/handle-webhook.md), [Response conventions](../domains/shared/response-conventions.md), [Error handling](../domains/shared/error-handling.md), [Integration checklist](../references/crypto/integration-checklist.md) | 说明如何消费当前已发布数币能力集的异步通知。 |

## Before code generation

在生成代码前，请确保提示词或实现方案里已经包含：

- 目标能力路径
- 必填签名请求头
- 对应的共享规则文档
- 字段与状态所需的参考资料

对于当前已发布的数币能力集，可用端点包括：

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`
