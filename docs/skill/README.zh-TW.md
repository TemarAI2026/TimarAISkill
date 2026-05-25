# AI Integration Docs

## Positioning

本目錄是面向 AI 編程助手與外部接入方的公開入口，用於幫助讀者理解以 MCP 為底座的開放能力模型、目前已發布的業務領域，以及實作前的建議閱讀順序。

## MCP-first model

- `MCP` 是執行期開放能力的主要底座。
- `skill` 是面向 AI 的引導與觸發層。
- `x402` 與後續其他支付協議應作為 MCP 之上的適配層，而不是各自維護獨立業務核心。

## Who this is for

- 使用 Claude Code、Cursor 或 Codex 協助接入支付能力的開發者
- 需要快速定位能力文件、參考資料與實作約束的外部接入方
- 希望在產生程式碼前先建立正確上下文的工程團隊

## Supported AI coding assistants

- Claude Code
- Cursor
- Codex

## Quick start

建議在產生或修改整合程式碼前，按以下順序閱讀：

1. 從 [`hub/integration-router.zh-TW.md`](./hub/integration-router.zh-TW.md) 開始，確認對應的 MCP 能力路徑與建議閱讀順序。
2. 繼續閱讀 [`hub/domain-map.zh-TW.md`](./hub/domain-map.zh-TW.md)，了解目前已發布業務領域與文件布局。
3. 進入目前業務領域文件。目前已發布的執行期業務領域為 [`domains/crypto/overview.zh-TW.md`](./domains/crypto/overview.zh-TW.md)。
4. 再查閱共享規則文件，例如 [`domains/shared/auth-signing.zh-TW.md`](./domains/shared/auth-signing.zh-TW.md)、[`domains/shared/error-handling.zh-TW.md`](./domains/shared/error-handling.zh-TW.md) 與 [`domains/shared/response-conventions.zh-TW.md`](./domains/shared/response-conventions.zh-TW.md)。
5. 最後讀取參考資料，再開始產生程式碼，例如 [`references/crypto/integration-checklist.zh-TW.md`](./references/crypto/integration-checklist.zh-TW.md)、[`references/crypto/endpoints.zh-TW.md`](./references/crypto/endpoints.zh-TW.md) 與 [`references/shared/headers.zh-TW.md`](./references/shared/headers.zh-TW.md)。

## Current domain availability

- `Crypto`：目前已發布，可用於基於 MCP 的整合規劃與程式碼產生。
- `Fiat`：已保留為未來業務領域入口，目前不應視為已實作。

## Directory guide

- [`hub/`](./hub/)：總覽入口、路由說明與 MCP 能力導航。
- [`domains/`](./domains/)：按目前已發布業務領域組織的實作說明。
- [`domains/crypto/`](./domains/crypto/)：目前可用的數幣能力文件。
- [`domains/fiat/`](./domains/fiat/)：未來法幣文件的保留位置。
- [`domains/shared/`](./domains/shared/)：認證、錯誤處理、回應慣例等跨能力共享規則。
- [`references/`](./references/)：端點、模型、狀態、請求標頭與檢查清單等參考資料。
- [`examples/`](./examples/)：用於輔助理解實作的範例資產。

## Important usage note

這些文件的目標是在程式碼產生前建立正確上下文。請先閱讀 router 與目前已發布業務領域說明，再結合共享規則和參考資料實作程式碼；不要只根據單一端點頁面直接產生整合邏輯。
