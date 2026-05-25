# Domain Map

## Current document layers

公開整合文件按少量層次組織，方便讀者從入口逐步走到實作細節：

- Hub 文件，例如 [Integration Router](./integration-router.zh-TW.md)，用來幫助選擇閱讀路徑。
- Shared 共享文件，例如 [Auth Signing](../domains/shared/auth-signing.zh-TW.md)、[Error Handling](../domains/shared/error-handling.zh-TW.md) 與 [Response Conventions](../domains/shared/response-conventions.zh-TW.md)，說明跨能力重用的規則。
- Domain 業務領域文件，例如 [Crypto Overview](../domains/crypto/overview.zh-TW.md)，說明特定業務領域下目前已發布的執行期能力流程。
- Reference 參考文件，例如 [crypto endpoints](../references/crypto/endpoints.zh-TW.md)、[request and response models](../references/crypto/request-response-models.zh-TW.md) 與 [shared headers](../references/shared/headers.zh-TW.md)，提供實作細節。
- Examples 範例文件，例如 [Node.js crypto example](../examples/crypto/nodejs/README.md)、[C# crypto example](../examples/crypto/csharp/README.md) 與 [cURL crypto example](../examples/crypto/curl/README.md)，展示端到端使用方式。

## MCP-first interpretation

- MCP 是目前公開文件背後的能力底座。
- `skill`、`x402` 與未來其他協議面應路由進 MCP，而不是各自維護獨立業務核心。
- 目前的 `docs/skill` 樹應理解為「如何使用基於 MCP 的能力」的公開引導層。

## Available domains

- Shared：在任何業務流程之前或過程中都可能用到的跨能力規則，包括簽名、回應慣例與錯誤處理。
- Crypto：目前已發布業務領域，涵蓋已公開數幣端點的支付、代付、訂單查詢、Webhook 與餘額能力說明。
- Fiat：保留給未來的業務領域。入口文件已存在於 [Fiat README](../domains/fiat/README.zh-TW.md)，但目前尚未發布法幣流程、欄位集合或狀態映射。

## Expansion note

未來新增業務領域時，Shared 層仍作為共同基礎，而各業務領域再補上自己的指南、參考文件與範例。外部接入方與 AI 編碼助手應先從 hub 開始，確認能力路徑後，再依序閱讀對應的 shared 與 domain 文件，之後再產生程式碼。
