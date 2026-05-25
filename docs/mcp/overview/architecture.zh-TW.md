# MCP Architecture

## MCP role

MCP 是 Timar 平台開放能力的統一底座。

它的職責是：

- 提供一致的開放能力入口
- 統一執行期呼叫模型
- 統一簽名、請求識別、錯誤語義與能力路由

## Relationship to upper layers

- `skill`：AI-facing 的引導與觸發層
- `x402`：協議適配層
- 未來其他協議：同樣應作為 MCP 上層適配層

這些上層入口不應各自維護獨立業務核心，而應路由進 MCP。

## What MCP owns in phase one

- payment capability path
- payout capability path
- balance capability path
- notification capability path
- common runtime calling conventions

## What MCP does not own in phase one

- 商戶註冊流程
- KYB/KYC 過程本身
- 內部錢包實作細節
- settlement / ledger 內部邏輯
- 內部對帳與營運流程

這些要麼屬於前置條件，要麼屬於下層平台實作。
