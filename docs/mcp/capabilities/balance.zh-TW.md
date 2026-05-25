# 餘額能力

## 適用範圍

第一階段 MCP 的 balance capability 目前覆蓋已發布的數幣帳戶餘額查詢。

## 目前介面族

- `GET /api/v2/digital/balances`

## 呼叫前需要準備什麼

呼叫此能力前，應先具備以下前提：

- 執行環境、`apiKey`、`secretKey` 與簽名邏輯已配置好
- 呼叫方已明確本次查餘額的用途，例如後台展示、代付前置校驗或對帳
- 下游程式碼能區分「帳戶視圖資料」與「訂單狀態資料」

## 回應概念

目前公開餘額回應會返回一個或多個餘額項，核心欄位包括：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

`availableBalance` 表示仍可用於新操作的餘額，`lockedBalance` 表示已被預留或凍結的餘額，`totalBalance` 是兩者之和。

## 使用說明

- 應按 `currency` 維度消費餘額
- 只允許使用可用餘額的場景，不要誤用 `totalBalance`
- 不要把一次餘額讀取當成代付並發控制的唯一依據
- 若餘額會影響資金決策，建議保留足夠的快照或日誌，便於排查與對帳

## 常見錯誤

- 把 `totalBalance` 當成全部可用餘額
- 忽略 `lockedBalance`
- 用查餘額取代業務側的資金預留邏輯
- 把餘額資料和訂單查詢結果混在同一套處理邏輯裡

## 精確參考

- [`../../skill/domains/crypto/query-balance.zh-TW.md`](../../skill/domains/crypto/query-balance.zh-TW.md)
- [`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
- [`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)
- [`../../skill/references/crypto/integration-checklist.zh-TW.md`](../../skill/references/crypto/integration-checklist.zh-TW.md)
