# MCP Public Docs

## Positioning

本目錄是 Timar 平台面向外部接入方與 AI 編程助手的 MCP 公共文件入口。

在目前架構中：

- `MCP` 是開放能力底座
- `skill` 是面向 AI 的引導與觸發層
- `x402` 與未來協議面是 MCP 上層適配層

## Who this is for

- 需要接入 Timar 開放能力的外部商戶或工程團隊
- 使用 Claude Code、Cursor、Codex 等 AI 編程助手的開發者
- 需要快速理解 MCP 能力邊界與呼叫路徑的解決方案工程師

## Runtime prerequisites

本目錄預設以下前置條件已經完成：

- 商戶註冊與權限開通
- 必要的業務准入與內部審核
- API 憑證已經發放

執行期接入重點不在開戶流程，而在：

- 選擇環境
- 配置 `apiKey` 與 `secretKey`
- 產生簽名相關輸入
- 呼叫 MCP 能力

## Reading order

1. 先讀 [`overview/architecture.zh-TW.md`](./overview/architecture.zh-TW.md)
2. 再讀 [`overview/environments.zh-TW.md`](./overview/environments.zh-TW.md)
3. 再讀 [`overview/auth-signing.zh-TW.md`](./overview/auth-signing.zh-TW.md)
4. 然後進入能力頁：
   - [`capabilities/payment.zh-TW.md`](./capabilities/payment.zh-TW.md)
   - [`capabilities/payout.zh-TW.md`](./capabilities/payout.zh-TW.md)
   - [`capabilities/balance.zh-TW.md`](./capabilities/balance.zh-TW.md)
   - [`capabilities/notifications.zh-TW.md`](./capabilities/notifications.zh-TW.md)
5. 最後使用 [`references/common.zh-TW.md`](./references/common.zh-TW.md) 並結合現有 `docs/skill/references/` 進行精確契約核對

## Relationship to `docs/skill`

`docs/mcp/` 是 MCP-first 的公共入口層。  
`docs/skill/` 會繼續保留，作為 AI-facing 的能力引導層與目前已發布數幣能力說明層。

在第一階段中：

- `docs/mcp/` 負責總體說明與能力路由
- `docs/skill/references/` 繼續承載目前已發布能力的精確事實層
