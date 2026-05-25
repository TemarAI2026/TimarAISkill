# Integration Router

## How to use this router

將此頁作為撰寫整合程式或提示 AI 程式助理前的第一站。先根據要完成的任務打開對應的共用文件與領域文件，再到參考文件確認實際的請求與回應欄位。

如果你是第一次接觸這組 API，請先閱讀 [Domain Map](./domain-map.zh-TW.md)，再回到本頁。

## Task-based routes

| Task | Read next | Why this path |
| --- | --- | --- |
| Sign a request | [Shared signing guide](../domains/shared/auth-signing.zh-TW.md), [Shared headers reference](../references/shared/headers.zh-TW.md), [Signature examples](../references/shared/signature-examples.zh-TW.md) | 說明共用簽名流程，以及必填標頭：`X-Api-Key`、`X-Api-Timestamp`、`X-Api-RequestId`、`X-Api-Sign`。 |
| Create a crypto payment | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Create payment](../domains/crypto/create-payment.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Request and response models](../references/crypto/request-response-models.zh-TW.md) | 對應 `POST /api/v2/digital/payments` 的支付流程，並提供實作所需的請求與回應欄位。 |
| Query a payment order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Crypto statuses](../references/crypto/statuses.zh-TW.md) | 對應 `GET /api/v2/digital/payments/{orderId}` 的訂單查詢，並協助解讀回傳的訂單狀態。 |
| Cancel a payment order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Error handling](../domains/shared/error-handling.zh-TW.md) | 對應 `POST /api/v2/digital/payments/{orderId}/cancel`，包含取消前後的狀態確認與錯誤處理重點。 |
| Create a crypto payout | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Create payout](../domains/crypto/create-payout.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Request and response models](../references/crypto/request-response-models.zh-TW.md) | 對應 `POST /api/v2/digital/payouts` 的代付流程，涵蓋送單與追蹤所需欄位。 |
| Query a payout order | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query order](../domains/crypto/query-order.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md), [Crypto statuses](../references/crypto/statuses.zh-TW.md) | 對應 `GET /api/v2/digital/payouts/{orderId}`，幫助你以一致方式理解代付生命週期狀態。 |
| Query balance | [Crypto overview](../domains/crypto/overview.zh-TW.md), [Query balance](../domains/crypto/query-balance.zh-TW.md), [Crypto endpoints](../references/crypto/endpoints.zh-TW.md) | 對應 `GET /api/v2/digital/balances` 的餘額查詢，以及相關的加密貨幣帳務語境。 |
| Understand callback and webhook handling | [Handle webhook](../domains/crypto/handle-webhook.zh-TW.md), [Response conventions](../domains/shared/response-conventions.zh-TW.md), [Error handling](../domains/shared/error-handling.zh-TW.md), [Integration checklist](../references/crypto/integration-checklist.zh-TW.md) | 說明如何接收、驗證並處理非同步加密貨幣通知，協助建立較完整的正式環境流程。 |

## Before code generation

開始產生程式碼前，請確認提示或實作計畫中已包含目標端點、必填簽名標頭、對應任務的領域文件，以及用來確認欄位與狀態的參考文件。

目前已公開的加密貨幣整合端點如下：

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`
