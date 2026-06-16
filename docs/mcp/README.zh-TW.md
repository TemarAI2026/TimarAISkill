# MCP 公開文件

## 定位

此目錄是 Temar 平台面向外部接入方與 AI 編程助手的 MCP 公開文件入口。

在目前架構中：

- `MCP` 是開放能力底座
- `skill` 是面向 AI 的引導與觸發層
- `x402` 以及未來其他協議面，都是建立在 MCP 之上的上層適配層

如果未來有新的接入面需要重用收單、代付或公共簽名邏輯，應優先路由進 MCP，而不是再維護一套獨立業務核心。

## 面向對象

- 需要對接 Temar 開放能力的外部商戶或工程團隊
- 使用 Codex、Claude Code、Cursor 等 AI 編程助手做接入的開發者
- 需要快速理解 MCP 能力邊界與呼叫路徑的解決方案工程師

## 這些文件預設你已具備的前提

這些文件預設在開始執行期接入前，以下工作已經完成：

- 商戶註冊與接入開通
- 必要的業務准入與內部審核
- API 憑證已經簽發

執行期階段真正關注的是：

- 選擇目標環境
- 配置 `apiKey` 與 `secretKey`
- 產生簽名輸入
- 呼叫 MCP 能力
- 正確保存返回的識別值與狀態

## 快速接入路徑

如果你是從 0 開始接入，建議依這個順序閱讀：

1. 先讀 [`overview/architecture.zh-TW.md`](./overview/architecture.zh-TW.md)，理解 MCP 管什麼、不管什麼
2. 再讀 [`overview/environments.zh-TW.md`](./overview/environments.zh-TW.md)，理解環境隔離與執行期假設
3. 在產生任何客戶端程式碼前，先讀 [`overview/auth-signing.zh-TW.md`](./overview/auth-signing.zh-TW.md)
4. 再依需要進入能力頁：
   - [`capabilities/payment.zh-TW.md`](./capabilities/payment.zh-TW.md)
   - [`capabilities/payout.zh-TW.md`](./capabilities/payout.zh-TW.md)
   - [`capabilities/balance.zh-TW.md`](./capabilities/balance.zh-TW.md)
   - [`capabilities/notifications.zh-TW.md`](./capabilities/notifications.zh-TW.md)
5. 最後結合 [`references/common.zh-TW.md`](./references/common.zh-TW.md) 與目前精確 reference 頁面做契約核對

## 第一階段能力範圍

目前公開 MCP 能力範圍包括：

- 收單
- 代付
- 餘額
- 通知
- 公共執行期呼叫約定

目前第一階段不納入公開 MCP 主範圍的內容包括：

- 商戶註冊流程
- KYB / KYC 處理流程
- settlement / ledger 內部實作
- wallet 內部實作細節
- 內部對帳與營運處置流程
- 已發布回應契約之外的鏈上執行細節

## 執行期五步檢查

無論是人工接入，還是讓 AI 編程助手產生程式碼，都建議按這個順序推進：

1. 先確認目標環境與憑證
2. 再確認請求標頭與簽名輸入
3. 再確認具體能力的請求欄位與端點路徑
4. 正確保存平台返回的識別值、狀態值與追蹤資訊
5. 為查單、重試、取消與異步通知準備安全的後續處理邏輯

## 常見錯誤

- 把 `docs/mcp/` 當成商戶開戶文件，而不是執行期接入文件
- 自行臆造目前公開模型中不存在的請求欄位
- 把收單與代付混成一套泛化流程
- 忽略 create 介面返回的平台 `orderId`
- 只看摘要說明，不回到精確 reference 做最終核對

## 與 `docs/skill` 的關係

`docs/mcp/` 是 MCP-first 的公開入口層。
`docs/skill/` 繼續保留，作為面向 AI 的引導層，以及目前已發布數幣能力的精確 reference 層。

在目前階段：

- `docs/mcp/` 負責整體說明、接入路徑與能力導航
- `docs/skill/references/` 繼續承載目前已發布能力的精確事實層

建議的用法是：先從 `docs/mcp/` 入門，再回到精確 reference 完成實作與上線前核對。
