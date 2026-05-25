# AI Integration Docs

## Positioning

本目錄是面向 AI 編碼助理與外部整合方的公開入口，用來快速理解 VGPAY 的整合文件結構、可用業務領域與建議閱讀順序。

## Who this is for

- 使用 Claude Code、Cursor 或 Codex 協助整合支付能力的開發者
- 需要快速定位業務領域文件、參考資料與實作約束的外部整合方
- 希望在產生程式碼前先建立正確上下文的工程團隊

## Supported AI coding assistants

- Claude Code
- Cursor
- Codex

## Quick start

建議在產生或修改整合程式碼前，依照以下順序閱讀：

1. 先從 [`hub/integration-router.zh-TW.md`](./hub/integration-router.zh-TW.md) 開始，確認任務入口與建議閱讀路徑。
2. 接著閱讀 [`hub/domain-map.zh-TW.md`](./hub/domain-map.zh-TW.md)，了解目前業務領域與文件分布。
3. 進入目前的業務領域文件。當前已實作的業務領域為 [`domains/crypto/overview.zh-TW.md`](./domains/crypto/overview.zh-TW.md)。
4. 再查閱共用規則文件，例如 [`domains/shared/auth-signing.zh-TW.md`](./domains/shared/auth-signing.zh-TW.md)、[`domains/shared/error-handling.zh-TW.md`](./domains/shared/error-handling.zh-TW.md) 與 [`domains/shared/response-conventions.zh-TW.md`](./domains/shared/response-conventions.zh-TW.md)。
5. 最後閱讀參考資料，再開始產生程式碼，例如 [`references/crypto/integration-checklist.zh-TW.md`](./references/crypto/integration-checklist.zh-TW.md)、[`references/crypto/endpoints.zh-TW.md`](./references/crypto/endpoints.zh-TW.md) 與 [`references/shared/headers.zh-TW.md`](./references/shared/headers.zh-TW.md)。

## Current domain availability

- `Crypto`：目前已實作，可用於整合規劃與程式碼產生。
- `Fiat`：已保留作為未來的業務領域入口，目前不應視為已實作。

## Directory guide

- [`hub/`](./hub/)：總覽入口、路由說明與業務領域映射。
- [`domains/`](./domains/)：依業務領域整理的實作說明。
- [`domains/crypto/`](./domains/crypto/)：目前可用的加密貨幣整合文件。
- [`domains/fiat/`](./domains/fiat/)：法幣業務領域的保留位置。
- [`domains/shared/`](./domains/shared/)：跨業務領域共用的驗證、錯誤處理與回應慣例。
- [`references/`](./references/)：介面端點、資料模型、狀態、標頭與整合檢查清單等參考資料。
- [`examples/`](./examples/)：可補充實作理解的示例資產目錄。

## Important usage note

這些文件的目標是幫助你先建立正確上下文，再進行程式碼產生。請先閱讀路由與業務領域說明，再結合共用規則與參考資料實作程式碼；不要只根據單一介面頁面直接產生整合邏輯。
