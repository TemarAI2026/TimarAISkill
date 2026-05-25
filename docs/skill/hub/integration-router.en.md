# Integration Router

## How to use this router

Use this page as the first stop before writing integration code or prompting an AI coding assistant. The purpose of this router is to help you choose the right MCP-backed capability path, then open the linked shared and published domain guides before implementation.

If you are new to the public API surface, read [Domain Map](./domain-map.en.md) first, then return here.

## Capability-based routes

| Task | Read next | Why this path |
| --- | --- | --- |
| Sign a request | [Shared signing guide](../domains/shared/auth-signing.en.md), [Shared headers reference](../references/shared/headers.en.md), [Signature examples](../references/shared/signature-examples.en.md) | Explains the shared signing flow and the required headers for MCP-backed capability calls. |
| Create a crypto payment | [Crypto overview](../domains/crypto/overview.en.md), [Create payment](../domains/crypto/create-payment.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md), [Request and response models](../references/crypto/request-response-models.en.md) | Covers the published payment capability path currently exposed through the crypto domain docs. |
| Query a payment order | [Crypto overview](../domains/crypto/overview.en.md), [Query order](../domains/crypto/query-order.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md), [Crypto statuses](../references/crypto/statuses.en.md) | Helps interpret the published payment-query capability path and returned order states. |
| Cancel a payment order | [Crypto overview](../domains/crypto/overview.en.md), [Query order](../domains/crypto/query-order.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md), [Error handling](../domains/shared/error-handling.en.md) | Guides the published payment-cancel capability path and its error-handling expectations. |
| Create a crypto payout | [Crypto overview](../domains/crypto/overview.en.md), [Create payout](../domains/crypto/create-payout.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md), [Request and response models](../references/crypto/request-response-models.en.md) | Covers the published payout capability path for the current crypto domain. |
| Query a payout order | [Crypto overview](../domains/crypto/overview.en.md), [Query order](../domains/crypto/query-order.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md), [Crypto statuses](../references/crypto/statuses.en.md) | Applies to the published payout-query capability path and the current payout lifecycle meanings. |
| Query balance | [Crypto overview](../domains/crypto/overview.en.md), [Query balance](../domains/crypto/query-balance.en.md), [Crypto endpoints](../references/crypto/endpoints.en.md) | Covers the published balance capability path and the surrounding crypto account context. |
| Understand callback and webhook handling | [Handle webhook](../domains/crypto/handle-webhook.en.md), [Response conventions](../domains/shared/response-conventions.en.md), [Error handling](../domains/shared/error-handling.en.md), [Integration checklist](../references/crypto/integration-checklist.en.md) | Explains how to consume asynchronous notifications for the current published crypto capability set. |

## Before code generation

Before generating code, make sure the prompt or implementation plan includes:

- the target capability path
- the required signing headers
- the shared rule documents
- the matching reference documents for fields and statuses

For the currently published crypto capability set, the available endpoints are:

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`
