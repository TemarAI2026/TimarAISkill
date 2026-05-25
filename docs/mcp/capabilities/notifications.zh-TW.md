# 通知能力

## 適用範圍

第一階段 MCP 的 notification capability 覆蓋的是異步訂單通知的接收側處理。它的目標不是臆造一套 webhook 協議，而是約束接入方與 AI 助手如何圍繞已發布契約實作一個安全的接收器。

## 第一階段目前要求

- 正確接收異步通知
- 做好冪等處理
- 將通知資料與本地訂單記錄核對
- 在需要時結合查單結果與目前狀態語義完成業務更新

## 實作前先確認什麼

在實作通知處理前，應先確認目前已發布整合契約中的這些部分：

- 發送方身份
- 投遞路徑
- 請求標頭
- 簽名或驗簽規則
- 載荷結構
- 重試行為
- 成功回應要求

若這些點沒有被清楚發布，就不要在 MCP 層自行猜測。

## 接收器實作說明

- 記錄穩定的請求追蹤識別
- 保留原始請求體與關鍵請求標頭，便於排查
- 圍繞穩定事件鍵，或「訂單 + 狀態流轉」設計冪等邏輯
- 更新本地狀態前，先將訂單號、金額、幣種、網路等關鍵值與自身記錄核對
- 只有在公開契約明確說明時，才把通知載荷直接視為最終權威

## 常見錯誤

- 在公開契約未定義時，自行發明 webhook 欄位或驗簽規則
- 對重複通知進行多次業務處理
- 在未核對通知值前，直接更新本地業務狀態
- 在業務規則仍要求補充查單時，只憑通知成功就認為證據已足夠

## 精確參考

- [`../../skill/domains/crypto/handle-webhook.zh-TW.md`](../../skill/domains/crypto/handle-webhook.zh-TW.md)
- [`../../skill/domains/shared/response-conventions.zh-TW.md`](../../skill/domains/shared/response-conventions.zh-TW.md)
- [`../../skill/domains/shared/error-handling.zh-TW.md`](../../skill/domains/shared/error-handling.zh-TW.md)
- [`../../skill/references/crypto/integration-checklist.zh-TW.md`](../../skill/references/crypto/integration-checklist.zh-TW.md)
