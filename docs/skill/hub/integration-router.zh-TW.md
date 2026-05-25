# Integration Router

## How to use this router

使用本頁作為撰寫整合程式碼或提示 AI 編程助手前的第一站。這個 router 的目標，是幫助你先選擇正確的 MCP 能力路徑，再進入對應的共享規則與目前已發布業務領域說明。

如果你剛接觸目前公開 API 面，請先讀 [Domain Map](./domain-map.zh-TW.md)，再回到這裡。

## Capability-based routes

| Task | Read next | Why this path |
| --- | --- | --- |
| Sign a request | [Shared signing guide](../domains/shared/auth-signing.zh-TW.md), [Shared headers reference](../references/shared/headers.zh-TW.md), [Signature examples](../references/shared/signature-examples.zh-TW.md) | 說明 MCP 背後的共享簽名流程，以及呼叫能力時需要的請求標頭。 |
| Create a crypto payment | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Create payment](../domains/crypto/create-payment.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Request and response models](../references/crypto/request-response-models.zh-TW.md) | 覆蓋目前透過數幣業務領域文件公開的支付能力路徑。 |
| Query a payment order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Crypto statuses](../references/crypto/statuses.zh-TW.md) | 幫助理解目前公開的支付查單能力路徑與回傳狀態。 |
| Cancel a payment order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Error handling](../domains/shared/error-handling.zh-TW.md) | 指向目前公開的支付取消能力路徑及其錯誤處理要求。 |
| Create a crypto payout | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Create payout](../domains/crypto/create-payout.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Request and response models](../references/crypto/request-response-models.zh-TW.md) | 覆蓋目前數幣業務領域公開的代付能力路徑。 |
| Query a payout order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Crypto statuses](../references/crypto/statuses.zh-TW.md) | 適用於目前公開的代付查單能力路徑與狀態含義。 |
| Query balance | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query balance](../domains/crypto/query-balance.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md) | 覆蓋目前公開的餘額能力路徑及其數幣帳戶上下文。 |
| Understand callback and webhook handling | [Handle webhook](../domains/crypto/handle-webhook.zh-TW.md), [Response conventions](../domains/shared/response-conventions.zh-TW.md), [Error handling](../domains/shared/error-handling.zh-TW.md), [Integration checklist](../references/crypto/integration-checklist.zh-TW.md) | 說明如何消費目前已發布數幣能力集的非同步通知。 |

## Before code generation

在產生程式碼前，請確保提示詞或實作方案裡已經包含：

- 目標能力路徑
- 必填簽名請求標頭
- 對應的共享規則文件
- 欄位與狀態所需的參考資料

對於目前已發布的數幣能力集，可用端點包括：

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`
