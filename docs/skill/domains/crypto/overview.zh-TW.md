# 加密貨幣總覽

## 這個領域涵蓋什麼

Crypto 領域面向目前公開的數位資產整合能力，幫助外部整合方與 AI 編碼助手理解現行 V2 端點、欄位約束，以及支付與代付流程的核心差異。

目前公開端點包括：

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`

## 支援的任務

- 建立加密貨幣支付訂單，並取得 `paymentUrl`、`receiveAddress`、`expiresInSeconds`
- 查詢支付訂單狀態與支付明細
- 取消尚未完成的支付訂單
- 建立加密貨幣代付訂單，並追蹤提幣結果
- 查詢帳戶餘額，包括 `availableBalance`、`lockedBalance`、`totalBalance`
- 規劃 webhook 接收端，但在實作前先確認最新公開文件與你的整合約定

## 支付與代付的差異

支付是收款流程，入口為 `POST /api/v2/digital/payments`。建立後通常會使用 `paymentUrl` 或 `receiveAddress` 引導付款，並關注訂單是否在有效期限內完成。

代付是出款流程，入口為 `POST /api/v2/digital/payouts`。代付請求必須明確提供 `withdrawAddress`，並更重視地址正確性、餘額是否足夠，以及鏈上出款後的狀態追蹤。

不要混用這兩種流程的欄位或思路。例如，`returnUrl` 與 `cancelUrl` 只屬於目前的支付建立模型，`withdrawAddress` 只屬於目前的代付建立模型。

## 接下來讀什麼

- [建立支付](./create-payment.zh-TW.md)
- [建立代付](./create-payout.zh-TW.md)
- [查詢訂單](./query-order.zh-TW.md)
- [查詢餘額](./query-balance.zh-TW.md)
- [處理 webhook](./handle-webhook.zh-TW.md)
- [加密貨幣端點參考](../../references/crypto/endpoints.zh-TW.md)
- [請求與回應模型](../../references/crypto/request-response-models.zh-TW.md)
