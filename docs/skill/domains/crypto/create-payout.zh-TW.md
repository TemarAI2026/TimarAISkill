# 建立代付

## 何時使用這個端點

當你需要從平台向外部錢包位址發起一筆新的加密貨幣出款時，使用 `POST /api/v2/digital/payouts`。這不是收款流程的鏡像版本；它是獨立的出款流程，重點在出款位址、費用與後續狀態追蹤。

目前建立成功後，回應通常會包含 `orderId`、`merchantOrderId`、`merchantUserId`、`amount`、`fee`、`currency`、`network`、`withdrawAddress`。

## 需要確認的請求欄位

目前代付建立模型欄位如下：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 必填：`withdrawAddress`

在產生程式碼前，先確認：

- `withdrawAddress` 是否來自正確鏈路，並與 `network` 匹配
- `merchantOrderId` 在你的商戶端是否唯一
- `merchantUserId` 是否滿足你的稽核或使用者歸屬需求
- `amount` 與餘額策略是否已在你的業務層做過前置檢查

## 產生程式碼前

先把這些上下文放進實作說明或提示詞：

- 目標端點是 `POST /api/v2/digital/payouts`
- 目前請求模型不包含 `callbackUrl`
- 程式需要消費建立回應中的 `orderId`、`fee`、`withdrawAddress`
- 後續查詢請搭配 [查詢訂單](./query-order.zh-TW.md) 與 [狀態參考](../../references/crypto/statuses.zh-TW.md)
- 代付和支付是不同流程，不應共用支付專用欄位如 `returnUrl`、`cancelUrl`

建議同時閱讀：

- [加密貨幣端點參考](../../references/crypto/endpoints.zh-TW.md)
- [請求與回應模型](../../references/crypto/request-response-models.zh-TW.md)
- [查詢餘額](./query-balance.zh-TW.md)
- [錯誤處理](../shared/error-handling.zh-TW.md)

## 常見錯誤

- 漏傳 `withdrawAddress`，或位址與 `network` 不匹配
- 把支付接口和代付接口當成同一流程的不同狀態
- 在目前代付建立請求中加入 `callbackUrl`
- 重用支付場景中的 `returnUrl`、`cancelUrl`
- 只記錄 `merchantOrderId`，卻沒有保存平台回傳的 `orderId` 供後續查詢
