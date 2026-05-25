# 加密資產整合檢查清單

## 開發前

- 確認環境中已安全配置 `X-Api-Key` 與 `secretKey`，且 `secretKey` 不進入日誌。
- 實作四個必填請求標頭，以及基於原始請求本文字串的簽名邏輯。
- 為每次請求產生新的 `X-Api-RequestId`，並對其做短期去重。
- 為每個建立請求產生唯一的 `merchantOrderId`。
- 持久化 `merchantOrderId`、平台回傳的 `orderId`、`X-Api-RequestId`、請求時間戳，以及原始請求/回應。
- 將支付與代付建模為兩條獨立流程，不混用 `returnUrl`、`cancelUrl` 與 `withdrawAddress`。
- 先依目前狀態參考設計狀態歸一與未知值兜底。
- 代付流程在業務側預先規劃餘額檢查與失敗處理。

## 上線前

- 逐項核對目前設定的 6 個公開端點路徑與方法完全一致。
- 分別驗證有請求本文與無請求本文的簽名，並覆蓋 Unix 毫秒與 ISO8601 兩種時間戳格式。
- 驗證缺少請求標頭、簽名錯誤、`Timestamp expired.`、訂單不存在、商戶訂單號重複等情境的日誌與告警。
- 驗證建立支付後已保存 `paymentUrl`、`receiveAddress`、`expiresInSeconds` 與平台 `orderId`。
- 驗證支付查詢與取消流程會先查單，再按 `orderId` 呼叫取消端點。
- 驗證建立代付後會保存 `fee`、`withdrawAddress`，查單後會更新 `totalFee`、`networkFee`、`serviceFee`、`txId`、`sourceAddress`。
- 驗證餘額處理清楚區分 `availableBalance`、`lockedBalance`、`totalBalance`。
- 驗證重試邏輯不會盲目重送建立請求，並且能用已保存的 `X-Api-RequestId` 與 `orderId` 做排障。
