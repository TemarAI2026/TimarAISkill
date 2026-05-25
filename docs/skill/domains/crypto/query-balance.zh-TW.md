# 查詢餘額

## 這個端點會回傳什麼

使用 `GET /api/v2/digital/balances` 查詢目前帳戶的幣種餘額視圖。當前公開回應概念包括：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

其中，`availableBalance` 表示可用於新操作的餘額，`lockedBalance` 表示已被占用或凍結的餘額，`totalBalance` 是兩者合計。

## 如何安全使用

請把餘額查詢視為帳戶視圖，而不是單筆訂單結果。它適合用於：

- 提交代付前做餘額預檢
- 後台展示各幣種的可用餘額與凍結餘額
- 對帳或監控餘額變化趨勢

不要只依賴一次餘額讀取來決定高併發出款結果。你的業務層仍應處理併發提交、額度預留與失敗回滾等問題。

## 實作前先讀

實作前建議先確認：

- 你的程式會依 `currency` 維度處理餘額資料
- 你的介面或服務清楚區分 `availableBalance` 與 `totalBalance`
- 你已閱讀 [加密貨幣端點參考](../../references/crypto/endpoints.zh-TW.md)
- 你已閱讀 [請求與回應模型](../../references/crypto/request-response-models.zh-TW.md)
