# Domain Map

## Current document layers

公開整合文件以少量分層方式組織，方便讀者從入口逐步走到實作細節：

- Hub 文件，例如 [Integration Router](./integration-router.zh-TW.md)，用來幫助選擇閱讀路徑。
- Shared 領域文件，例如 [Auth Signing](../domains/shared/auth-signing.zh-TW.md)、[Error Handling](../domains/shared/error-handling.zh-TW.md) 與 [Response Conventions](../domains/shared/response-conventions.zh-TW.md)，說明可跨領域重用的規則。
- Domain 領域文件，例如 [Crypto Overview](../domains/crypto/overview.zh-TW.md)，說明特定領域的業務流程。
- Reference 參考文件，例如 [crypto endpoints](../references/crypto/endpoints.zh-TW.md)、[request and response models](../references/crypto/request-response-models.zh-TW.md) 與 [shared headers](../references/shared/headers.zh-TW.md)，提供實作細節。
- Examples 範例文件，例如 [Node.js crypto example](../examples/crypto/nodejs/README.md)、[C# crypto example](../examples/crypto/csharp/README.md) 與 [cURL crypto example](../examples/crypto/curl/README.md)，展示端到端使用方式。

## Available domains

- Shared: 任何業務流程之前或過程中都可能用到的跨領域規則，包括簽名、回應慣例與錯誤處理。
- Crypto: 目前對外公開的領域，涵蓋已發布數位資產端點的支付、代付、訂單查詢、Webhook 與餘額能力。
- Fiat: 保留給未來的領域。入口文件已存在於 [Fiat README](../domains/fiat/README.zh-TW.md)，但目前尚未公開法幣流程、欄位集合或狀態映射。

## Expansion note

未來新增領域時，Shared 層仍作為共同基礎，而各領域再補上自己的指南、參考文件與範例。外部整合方與 AI 編碼助手應先從 hub 開始，確認領域後，再依序閱讀對應的 shared 與 domain 文件，之後才生成程式碼。
