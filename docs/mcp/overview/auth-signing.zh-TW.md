# Authentication and Signing

## Runtime credentials

在第一階段中，MCP 執行期接入主要圍繞以下輸入：

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

## What to read next

本頁只負責 MCP 視角下的總說明。  
精確契約事實仍應回到目前已發布參考資料核對：

- [`../../skill/references/shared/headers.zh-TW.md`](../../skill/references/shared/headers.zh-TW.md)
- [`../../skill/references/shared/signature-examples.zh-TW.md`](../../skill/references/shared/signature-examples.zh-TW.md)
- [`../../skill/domains/shared/auth-signing.zh-TW.md`](../../skill/domains/shared/auth-signing.zh-TW.md)

## Runtime rule

在目前公開能力集中：

- 每次請求都應產生新的 `requestId`
- 簽名應基於目前請求實際送出的原始內容
- 不應在簽名後再重組或重新格式化請求體
