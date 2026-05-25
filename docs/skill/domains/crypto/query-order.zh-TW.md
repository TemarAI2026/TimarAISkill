# 查詢訂單

## 可以查詢什麼

這份指南同時適用於兩類查詢：

- 支付訂單：`GET /api/v2/digital/payments/{orderId}`
- 代付訂單：`GET /api/v2/digital/payouts/{orderId}`

支付查詢通常用來確認訂單狀態、已支付金額、收款地址、付款連結、有效期與入帳明細。代付查詢通常用來確認訂單狀態、手續費、鏈上交易號、出款位址與來源位址。

## 實作前先讀

在程式中請把支付查詢與代付查詢視為兩個獨立端點處理，即使它們都使用 `orderId` 路徑參數。依目前公開模型，兩者的狀態型別與擴充欄位並不相同。

實作前建議先確認：

- 你有保存建立回應回傳的 `orderId`，而不只保存 `merchantOrderId`
- 你已閱讀對應的 [狀態參考](../../references/crypto/statuses.zh-TW.md)
- 你已閱讀 [請求與回應模型](../../references/crypto/request-response-models.zh-TW.md)
- 取消支付前，先結合支付查詢確認當前訂單狀態，再呼叫 `POST /api/v2/digital/payments/{orderId}/cancel`

## 常見錯誤

- 用支付查詢邏輯直接解析代付回傳，或反過來
- 只依 `merchantOrderId` 建表，卻沒有保存平台 `orderId`
- 假設支付與代付使用完全相同的狀態值或狀態型別
- 尚未閱讀狀態文件，就把某個狀態直接當成終態或成功態
