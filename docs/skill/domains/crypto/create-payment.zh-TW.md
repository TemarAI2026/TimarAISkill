# 建立支付

## 何時使用這個端點

當你需要為使用者建立一筆新的加密貨幣收款訂單時，使用 `POST /api/v2/digital/payments`。這個端點適合需要產生付款連結、收款地址與訂單有效期資訊的流程。

目前建立成功後，回應通常會包含 `orderId`、`merchantOrderId`、`paymentUrl`、`amount`、`receiveAddress`、`currency`、`network`、`expiresInSeconds`。

## 需要確認的請求欄位

目前支付建立模型欄位如下：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 選填：`returnUrl`
- 選填：`cancelUrl`

在產生程式碼前，先確認：

- `merchantOrderId` 在你的商戶端是否唯一
- `merchantUserId` 是否對應你自己的使用者識別
- `amount`、`currency`、`network` 是否來自目前允許的業務設定
- `returnUrl` 與 `cancelUrl` 是否只是前端跳轉位址，而不是伺服器非同步通知位址

## 產生程式碼前

先把這些上下文放進實作說明或提示詞：

- 目標端點是 `POST /api/v2/digital/payments`
- 目前請求模型不包含 `callbackUrl`
- 程式需要消費建立回應中的 `orderId`、`paymentUrl`、`receiveAddress`、`expiresInSeconds`
- 後續查詢請搭配 [查詢訂單](./query-order.zh-TW.md) 與 [狀態參考](../../references/crypto/statuses.zh-TW.md)
- 簽名、錯誤處理與回應結構要結合共享文件一起實作

建議同時閱讀：

- [加密貨幣端點參考](../../references/crypto/endpoints.zh-TW.md)
- [請求與回應模型](../../references/crypto/request-response-models.zh-TW.md)
- [驗證與簽名](../shared/auth-signing.zh-TW.md)
- [回應慣例](../shared/response-conventions.zh-TW.md)

## 常見錯誤

- 把 `callbackUrl` 當作目前支付建立欄位使用
- 把 `merchantOrderId` 當作平台回傳的 `orderId`
- 漏傳 `network`，或把鏈名和幣種合併成同一個欄位
- 把 `returnUrl`、`cancelUrl` 當作 webhook 位址
- 建立後不處理 `expiresInSeconds`，導致過期訂單仍持續展示給使用者
