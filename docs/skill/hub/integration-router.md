# Integration Router

## How to use this router

Use this page as the first stop before writing integration code or prompting an AI coding assistant. Start with the task you need to complete, open the linked shared and domain guides, then confirm the exact request and response fields in the reference documents.

If you are new to the API surface, read [Domain Map](./domain-map.md) first, then return here.

## Task-based routes

| Task | Read next | Why this path |
| --- | --- | --- |
| Sign a request | [Shared signing guide](../domains/shared/auth-signing.md), [Shared headers reference](../references/shared/headers.md), [Signature examples](../references/shared/signature-examples.md) | Explains the shared signing flow and the required headers: `X-Api-Key`, `X-Api-Timestamp`, `X-Api-RequestId`, `X-Api-Sign`. |
| Create a crypto payment | [Crypto overview](../domains/crypto/overview.md), [Create payment](../domains/crypto/create-payment.md), [Crypto endpoints](../references/crypto/endpoints.md), [Request and response models](../references/crypto/request-response-models.md) | Covers the payment flow for `POST /api/v2/digital/payments` and the payload and response fields needed for implementation. |
| Query a payment order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Crypto statuses](../references/crypto/statuses.md) | Guides order lookup for `GET /api/v2/digital/payments/{orderId}` and helps interpret returned order states. |
| Cancel a payment order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Error handling](../domains/shared/error-handling.md) | Use this route for `POST /api/v2/digital/payments/{orderId}/cancel`, including status checks and error handling expectations. |
| Create a crypto payout | [Crypto overview](../domains/crypto/overview.md), [Create payout](../domains/crypto/create-payout.md), [Crypto endpoints](../references/crypto/endpoints.md), [Request and response models](../references/crypto/request-response-models.md) | Covers the payout flow for `POST /api/v2/digital/payouts` and the fields needed to submit and track a payout request. |
| Query a payout order | [Crypto overview](../domains/crypto/overview.md), [Query order](../domains/crypto/query-order.md), [Crypto endpoints](../references/crypto/endpoints.md), [Crypto statuses](../references/crypto/statuses.md) | Applies to `GET /api/v2/digital/payouts/{orderId}` and helps map payout lifecycle states in a consistent way. |
| Query balance | [Crypto overview](../domains/crypto/overview.md), [Query balance](../domains/crypto/query-balance.md), [Crypto endpoints](../references/crypto/endpoints.md) | Covers balance retrieval with `GET /api/v2/digital/balances` and the surrounding crypto account context. |
| Understand callback and webhook handling | [Handle webhook](../domains/crypto/handle-webhook.md), [Response conventions](../domains/shared/response-conventions.md), [Error handling](../domains/shared/error-handling.md), [Integration checklist](../references/crypto/integration-checklist.md) | Explains how to receive, validate, and process asynchronous crypto notifications in a production-ready way. |

## Before code generation

Before generating code, make sure the prompt or implementation plan includes the target endpoint, required signing headers, the domain guide for the task, and the matching reference documents for fields and statuses.

For crypto integrations currently published, the available endpoints are:

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`
